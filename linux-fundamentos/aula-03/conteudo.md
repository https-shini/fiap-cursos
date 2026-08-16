# Aula 03 — Recursos Avançados

[← Voltar ao índice do curso](../README.md)

**Disciplina:** Linux Fundamentos
**Autor do material:** Henrique Poyatos
**Instituição:** FIAP ON

## Objetivos de Aprendizagem

Aprofundar o conhecimento sobre administração do sistema operacional Linux nos eixos de **gerenciamento de volumes de armazenamento**, **agendamento de tarefas** e **política de firewall**, incluindo a técnica avançada de segurança conhecida como *port-knocking*.

## Conceitos Fundamentais

- Volumes, partições e sistemas de arquivos
- Ferramentas de linha de comando: `fdisk`, `mkfs`, `mount`, `umount`, `fsck`
- Montagem automática via `/etc/fstab`
- Agendamento com `cron`, `anacron` e `crontab`
- Firewall com `iptables`: correntes (*chains*), ações e parâmetros
- Segurança adicional por meio de *port-knocking* com o serviço `knockd`

## Conteúdo

### 1. Gerenciamento de Volumes

#### 1.1 Conceito de volume

Qualquer unidade de armazenamento — HDD, SSD, pendrive, HD externo ou disco óptico (CD-ROM, DVD-ROM, Blu-ray) — é tratada pelo Linux como um **volume**. A principal diferença em relação ao Windows é a forma de organização:

| Windows | Linux (*nix) |
|---|---|
| Cada volume recebe uma letra (C:, D:, etc.) | Existe **uma única raiz** de diretórios (`/`) |
| Cada volume tem sua própria árvore de arquivos | Os volumes são "pendurados" (montados) em pontos específicos dessa árvore única |

Para fins didáticos, o material cria uma nova unidade de armazenamento virtual no **Oracle VirtualBox**, em uma instância Debian, com padrão **VDI**, alocação dinâmica e tamanho máximo de 8 GB — simulando a instalação de um segundo disco rígido físico.

#### 1.2 Partições

Um disco pode ser dividido logicamente em **partições**, por razões de:

- **Organização**: separar dados por finalidade.
- **Segurança**: aplicar regras de acesso distintas por partição.
- **Performance**: operações do sistema de arquivos costumam ser mais rápidas em partições menores.
- **Multiboot**: instalar sistemas operacionais diferentes no mesmo hardware físico.

**Tipos de partição:**
- **Partição primária**: até 4 por disco (limite histórico).
- **Partição estendida**: uma partição primária subdividida, com tabela de partições própria, contendo **partições lógicas**.

#### 1.3 Sistema de arquivos

Após a criação das partições, é preciso definir **como** a informação será fisicamente armazenada — tarefa do **sistema de arquivos**, componente do SO que organiza arquivos e diretórios em blocos de dados.

| Sistema Operacional | Sistemas de arquivo comuns |
|---|---|
| Windows | FAT32, exFAT (FAT64), NTFS |
| Linux | ext4 (mais usado), ReiserFS, Btrfs, XFS, ZFS |

- **FAT32**: fácil de implementar, compatível com quase todos periféricos, mas com desvantagens de segurança.
- **NTFS**: sistema mais seguro da Microsoft; usado nas instalações modernas do Windows.
- **ext4**: quarta versão do Extended File System, padrão no Linux atual.

#### 1.4 Identificação de discos — `/dev`

Todo periférico detectado pelo Linux vira um **pseudo arquivo** em `/dev`. Discos rígidos seguem o padrão `/dev/sd*`: `/dev/sda` (primeiro disco), `/dev/sdb` (segundo), e assim por diante.

```bash
sudo grep sdb /var/log/syslog*
ls -la /dev/sd*
```

No exemplo do material, o primeiro disco (`sda`) tem três partições: `/dev/sda1`, `/dev/sda2` e `/dev/sda5`. A numeração "pula" de 2 para 5 porque as partições 1 e 2 são primárias, e `sda5` é a primeira partição lógica dentro da estendida.

#### 1.5 Gerenciamento de partições — `fdisk`

```bash
sudo fdisk /dev/sda      # e digitar 'p' para exibir a tabela de partições
```

Observações sobre a saída (colunas "Início" e "Fim", em setores):

- A primeira partição nunca começa no setor 0; sempre inicia no setor **2048**. Os setores anteriores compõem o **MBR** (*Master Boot Record*), que armazena a tabela de partições e instruções de boot.
- Partições estendidas e suas partições lógicas ocupam praticamente os mesmos setores, pois a estendida é apenas um "contêiner".
- No exemplo, `/dev/sda1` contém o Linux instalado, e `/dev/sda5` é uma partição **swap**.

> **Swap**: extensão da memória RAM em disco. Quando a RAM está cheia, o sistema move dados para o disco (*swapping*) e vice-versa. Sem essa área, o SO se recusaria a abrir novos programas por falta de memória.

**Consultar o sistema de arquivos em uso:**
```bash
cat /etc/mtab | grep sda1
```
`/etc/mtab` lista todos os volumes atualmente montados, seus sistemas de arquivos e regras.

**Criar partições em um segundo disco (`/dev/sdb`):**
```bash
sudo fdisk /dev/sdb
```

| Tecla | Ação |
|---|---|
| `n` | Criar nova partição |
| `p` | Definir como primária |
| `+4G` | Definir tamanho (aceita sufixos K, M, G, T, P) |
| `t` + `L` | Alterar o tipo da partição |
| `p` | Exibir a tabela resultante |
| `w` | Gravar (*write*) e sair |
| `q` | Sair sem salvar |

```bash
ls /dev/sdb*     # confirma a criação de /dev/sdb1, /dev/sdb2
```

#### 1.6 Criar um sistema de arquivos — `mkfs`

```bash
sudo mkfs -t ext4 /dev/sdb1
sudo mkfs.ext4 /dev/sdb2      # atalho equivalente
```
`sudo mkfs.` + `[TAB]` lista as opções disponíveis (`ext2`, `ext3`, `ext4`, `ntfs`, `vfat`, `exfat`, `minix`, `msdos`, `cramfs`, `bfs`, etc.).

#### 1.7 Montar volumes — `mount`

```bash
sudo mkdir /media/novoDisco
sudo mount /dev/sdb1 /media/novoDisco/
df -h
```

> Mesmo uma partição nunca usada já aparece com alguma ocupação (ex.: 16 MB em 4 GB), pois o sistema de arquivos reserva espaço para funcionar corretamente.

O parâmetro `-o` do `mount` aceita opções específicas (ver quadro na seção 1.9).

#### 1.8 Desmontar volumes — `umount`

```bash
sudo umount /media/novoDisco
```

#### 1.9 Montagem automática — `/etc/fstab`

Para montar partições automaticamente no *boot*, registra-se no arquivo `/etc/fstab`.

**Estrutura das colunas do `/etc/fstab`:**

| Coluna | Nome | Descrição |
|---|---|---|
| 1 | Partição | Geralmente por **UUID** (ou LABEL), não pelo nome `/dev/sdX`, pois `/dev` sequer existe antes de a raiz ser montada |
| 2 | Ponto de montagem | Diretório onde o volume ficará disponível |
| 3 | Tipo | Sistema de arquivos (ex.: `ext4`) |
| 4 | Opções | Parâmetros de montagem (ver quadro abaixo) |
| 5 | Dump | Indica ao comando `dump` se deve copiar a partição em nível de bloco (`0` = não) |
| 6 | Ordem de verificação | Ordem em que o `fsck` verifica as partições no boot |

**Quadro 3.1 — Opções de montagem:**

| Opção | Descrição |
|---|---|
| `defaults` | Usa `rw`, `suid`, `dev`, `exec`, `async` |
| `rw` | Leitura e escrita |
| `ro` | Somente leitura |
| `exec` | Permite execução de binários |
| `noexec` | Impede execução de binários |
| `suid` | Permite bit SUID |
| `noauto` | Não monta automaticamente no boot |
| `user` | Permite que um usuário comum monte a partição |
| `owner` | Permite que o dono do dispositivo o monte |
| `nofail` | Não reporta erro caso a montagem falhe |

**Exemplo — adicionar entrada para montagem automática:**
```bash
su -
echo "/dev/sdb1 /media/novoDisco ext4 defaults 0 0" >> /etc/fstab
exit
```

**udev**: gerenciador dinâmico de dispositivos (sucessor da combinação DEVFS + hotplug), responsável pelo *plug and play*. Regras personalizadas em `/etc/udev/rules.d` permitem automatizar comportamentos, como montar um pendrive específico em local customizado.

#### 1.10 Verificar sistema de arquivos — `fsck`

```bash
sudo fsck /dev/sdb1
```
Verifica a integridade do sistema de arquivos, usado quando a partição está corrompida e não pode ser montada. Também é executado automaticamente no boot, como medida preventiva.

### 2. Agendamento de Tarefas

#### 2.1 Necessidade de automação

O **`cron`** (originado no Unix) permite executar tarefas de forma recorrente ou agendada, sem intervenção manual.

#### 2.2 Caso de uso: backup automático do diretório `/etc`

**Gerar backup com data dinâmica no nome:**
```bash
sudo tar cfz /var/backups/etc-backup-$(date +%Y-%m-%d).tar.gz /etc
```
O "truque" é executar `date` **dentro** do comando `tar`, permitindo nomes de arquivo únicos por execução (sem sobrescrever backups anteriores).

**Agendar com `crontab`:**
```bash
sudo crontab -u root -e
```

Sintaxe de cada linha (seis campos):
```
# m h dom mon dow   comando
  30 2 *   *   *    tar cfz /var/backups/etc-backup-$(date +%Y-%m-%d).tar.gz /etc
```

**Estrutura das colunas do crontab:**

| Coluna | Campo | Intervalo | Exemplo |
|---|---|---|---|
| 1 | Minutos | 0–59 | `30` = minuto 30; `*/5` = a cada 5 min |
| 2 | Hora | 0–23 | `5` = 5h; `*/20` = a cada 20 min (com demais `*`) |
| 3 | Dia do mês | 1–31 | `10` = todo dia 10 |
| 4 | Mês | 1–12 | `8` = agosto |
| 5 | Dia da semana | 0–6 (0=domingo, 6=sábado) | `3` = quarta-feira |
| 6 | Comando | — | — |

O asterisco (`*`) significa "todos os valores possíveis" naquele campo.

**Exemplos de expressões cron:**

| Expressão | Significado |
|---|---|
| `0 5 * * *` | Todos os dias, às 5h |
| `45 21 * * *` | Todos os dias, às 21h45 |
| `10 * * * *` | Todas as horas, no minuto 10 |
| `*/20 * * * *` | A cada 20 minutos |
| `30 23 10 * *` | Todo dia 10 do mês, às 23h30 |
| `0 2 8 8 *` | Todo dia 8 de agosto, às 2h |
| `15 4 * * 3` | Toda quarta-feira, às 4h15 |
| `30 2 * * *` | Todos os dias, às 2h30 (backup do `/etc`) |

Para salvar e sair do editor `vim`: `:x`.

#### 2.3 `anacron` e os diretórios `/etc/cron.*`

Alternativa simplificada: criar/vincular scripts em:

- `/etc/cron.hourly` — de hora em hora
- `/etc/cron.daily` — diariamente
- `/etc/cron.weekly` — semanalmente
- `/etc/cron.monthly` — mensalmente

Os horários exatos são definidos no arquivo global `/etc/crontab` (diferente do `crontab -e` por usuário), que exige informar explicitamente o **usuário** com o qual o comando será executado.

> O **`anacron`** complementa o `cron`, garantindo que tarefas atrasadas (por exemplo, porque a máquina estava desligada) sejam executadas assim que o sistema for religado — útil em notebooks e estações de trabalho não ligadas 24h.

### 3. Política de Firewall

#### 3.1 Introdução ao `iptables`

O **`iptables`** é o firewall mais utilizado no Linux, capaz de filtrar pacotes que entrem, saiam ou atravessem qualquer interface de rede.

**Correntes (chains) padrão:**

| Corrente | Descrição |
|---|---|
| `INPUT` | Pacotes destinados ao próprio sistema |
| `OUTPUT` | Pacotes originados no próprio sistema |
| `FORWARD` | Pacotes que apenas atravessam o sistema |
| `PREROUTING` / `POSTROUTING` | Usadas em NAT (**SNAT**: muda endereço de origem; **DNAT**: muda endereço de destino), comuns em *gateways* |

**Ações possíveis (`-j`):**

| Ação | Descrição |
|---|---|
| `ACCEPT` | Aceita o pacote |
| `REJECT` | Rejeita e devolve resposta ao remetente (ICMP tipo 3 para UDP; TCP reset para TCP) |
| `DROP` | Descarta silenciosamente, sem resposta |

**Principais parâmetros do `iptables`:**

| Parâmetro | Função |
|---|---|
| `-A` | Adiciona regra ao **final** da corrente |
| `-I` | Adiciona regra no **topo** da corrente |
| `-D` | Remove regra(s) |
| `-F` | Remove todas as regras |
| `-X` | Remove correntes definidas pelo usuário |
| `-P` | Define política padrão da corrente |
| `-L` | Lista as regras |
| `-i` | Interface de rede |
| `-p` | Protocolo (`tcp`, `udp`) |
| `-s` | IP de origem |
| `--dport` | Porta de destino |
| `-j` | Ação a aplicar |

> ⚠️ **Ordem das regras é crítica**: a primeira regra correspondente é aplicada e as demais são ignoradas. Regras de exceção (liberar um IP específico) devem vir **antes** de regras genéricas de bloqueio — usando `-I` (topo) ou posicionando manualmente antes da regra de bloqueio no script.

#### 3.2 Script de firewall — `/etc/firewall.sh`

**Primeira versão** — bloqueia todo o tráfego HTTP (porta 80) na interface `eth0`:

```bash
#!/bin/bash
iptables -F
iptables -X
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -A INPUT -i eth0 -p tcp --dport 80 -j REJECT
```

```bash
sudo chmod 700 /etc/firewall.sh   # apenas o root pode ler/escrever/executar (4+2+1=7)
sudo /etc/firewall.sh
sudo iptables -L
```

O acesso local (`links http://localhost`) continua funcionando, pois trafega pela interface `lo` (não `eth0`).

**Segunda versão** — libera um IP específico antes da regra geral de bloqueio:

```bash
#!/bin/bash
iptables -F
iptables -X
iptables -P INPUT ACCEPT
iptables -P FORWARD REJECT
iptables -P OUTPUT ACCEPT
iptables -A INPUT -i eth0 -s 179.209.93.204 -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -i eth0 -p tcp --dport 80 -j REJECT
```
Alternativa direta: `sudo iptables -I INPUT -i eth0 -s 179.209.93.204 -p tcp --dport 80 -j ACCEPT`

> **Limitação**: só é confiável com **IP dedicado**. Com IP dinâmico, uma reinicialização pode alterar o IP e invalidar a regra.

**Terceira versão** — modelo robusto com controle de estado (`conntrack`):

```bash
#!/bin/bash
iptables -F
iptables -X
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
iptables -A INPUT -j DROP
```
`--ctstate RELATED,ESTABLISHED` garante que conexões já em andamento (como a sessão SSH usada para configurar o firewall) não sejam derrubadas.

#### 3.3 Port-Knocking

**Conceito**: portas de serviço não precisam ficar permanentemente abertas. O **port-knocking** ("batidas na porta") libera uma porta apenas quando o cliente envia pacotes a uma sequência específica de portas, na ordem correta.

```bash
sudo apt install knockd
```

**Pré-requisito**: firewall em modo restritivo (regra `DROP` final), como na terceira versão do script.

**Configuração — `/etc/knockd.conf`:**
```ini
[options]
UseSyslog

[openSSH]
    sequence      = 7000,8000,9000
    seq_timeout   = 5
    command       = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
    tcpflags      = syn

[closeSSH]
    sequence      = 9000,8000,7000
    seq_timeout   = 5
    command       = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
    tcpflags      = syn
```

> ⚠️ **Correção crítica**: a regra que libera a porta deve usar `-I` (topo da corrente), não `-A`. Com `-A`, a regra ficaria após o `DROP` já existente e jamais seria alcançada.

**Habilitar o serviço — `/etc/default/knockd`:**
```ini
START_KNOCKD=1
```

```bash
sudo /etc/init.d/knockd start
```

**Testando:**
```bash
nmap 67.205.132.93 -p 22          # deve mostrar "filtered", não aberta
ssh usuario@67.205.132.93          # deve dar timeout (porta bloqueada)

# "Bater" na sequência correta:
for x in 7000 8000 9000; do
  nmap -Pn --host_timeout 201 --max-retries 0 -p $x 67.205.132.93
done

ssh usuario@67.205.132.93          # agora conecta normalmente
```

O `knockd` também pode **fechar** a porta automaticamente com a sequência inversa (9000, 8000, 7000), conforme `[closeSSH]`.

> **Recomendação**: alterar as portas padrão do exemplo (SSH de 22 para outra, em `/etc/ssh/sshd_config`; portas de knock de 7000/8000/9000 para valores fora do padrão), pois são as primeiras que um atacante tentará.

## Exemplos

| Cenário | Comando/Procedimento |
|---|---|
| Ver discos detectados | `sudo grep sdb /var/log/syslog*` e `ls -la /dev/sd*` |
| Listar partições de um disco | `sudo fdisk /dev/sda` → `p` |
| Criar partição de 4 GB | `sudo fdisk /dev/sdb` → `n` → `p` → `+4G` → `w` |
| Formatar em ext4 | `sudo mkfs.ext4 /dev/sdb1` |
| Montar partição | `sudo mount /dev/sdb1 /media/novoDisco/` |
| Montar automaticamente no boot | Entrada em `/etc/fstab` |
| Verificar integridade | `sudo fsck /dev/sdb1` |
| Backup diário automatizado | `crontab -e` + `tar` com `date` embutido |
| Bloquear porta 80 | `iptables -A INPUT ... --dport 80 -j REJECT` |
| Liberar SSH após knock | `knockd` + `iptables -I INPUT ... --dport 22 -j ACCEPT` |

## Aplicações Práticas

### Cheat sheet de comandos

```bash
# Volumes e partições
sudo fdisk /dev/sdX
sudo mkfs.ext4 /dev/sdX1
sudo mount /dev/sdX1 /ponto
sudo umount /ponto
sudo fsck /dev/sdX1
df -h
cat /etc/fstab
cat /etc/mtab

# Agendamento
sudo crontab -u root -e
cat /etc/crontab

# Firewall
sudo iptables -L
sudo iptables -F
sudo iptables -P INPUT ACCEPT
sudo iptables -A INPUT ...
sudo iptables -I INPUT ...
sudo iptables -D INPUT ...

# Port-knocking
sudo apt install knockd
sudo /etc/init.d/knockd start
nmap <IP> -p <porta>
```

## Boas Práticas

- Confirmar sempre o disco correto antes de particionar com `fdisk` — a operação é destrutiva.
- Usar **UUID** (ou LABEL) em vez do nome `/dev/sdX` no `/etc/fstab`, já que a numeração pode mudar entre boots.
- Nunca esquecer a regra de aceite para `lo` (loopback) ao configurar firewall remotamente — perder a sessão SSH atual pode significar perder o acesso ao servidor.
- A ordem das regras do `iptables` é determinante: regras específicas de liberação **antes** de regras genéricas de bloqueio.
- Usar `-I` em vez de `-A` quando a nova regra precisa ter prioridade sobre um bloqueio já existente (caso do `knockd.conf`).
- Evitar `chmod 777` em scripts sensíveis; usar permissões restritas (`700`), exclusivas do root.
- Trocar as portas padrão do `knockd` (7000, 8000, 9000) e do SSH (22) para dificultar ataques automatizados.
- Cuidado com regras de firewall baseadas em IP dinâmico: uma mudança de IP pode bloquear o próprio administrador.
- Testar sempre em ambiente controlado (VM) antes de aplicar regras de firewall em produção.

## Resumo

Esta aula consolidou três pilares da administração avançada de Linux: **gerenciamento de volumes** (particionamento com `fdisk`, formatação com `mkfs`, montagem manual/automática e verificação de integridade com `fsck`); **agendamento de tarefas** com `cron`/`crontab`, incluindo a sintaxe de seis campos e as alternativas simplificadas via `/etc/cron.*`; e **segurança de rede** com `iptables` (correntes, ações, ordem crítica das regras) reforçada pela técnica de **port-knocking** via `knockd`.

---

## Principais Pontos a Memorizar

- Um volume corresponde a como o Linux trata um disco (HDD/SSD); partições dividem esse volume; um sistema de arquivos (ex.: ext4) organiza os dados dentro da partição.
- Fluxo de provisionamento manual de um disco: `fdisk` (particionar) → `mkfs.ext4` (formatar) → `mount` (montar) → `fsck` (verificar integridade).
- `/etc/fstab` define montagem automática no boot; prefira **UUID**/LABEL em vez do nome `/dev/sdX`, que pode mudar entre boots.
- `cron`/`crontab` automatizam tarefas recorrentes com sintaxe de seis campos (minuto, hora, dia do mês, mês, dia da semana, comando); `/etc/cron.*` oferece atalhos simplificados via `anacron`.
- `iptables` organiza regras em correntes (*chains*) `INPUT`, `OUTPUT` e `FORWARD`; a ordem das regras é determinante — liberações específicas devem vir **antes** de bloqueios genéricos (`-I` insere no topo, `-A` acrescenta ao final).
- Nunca esquecer a regra de aceite para `lo` (loopback) ao configurar firewall remotamente, sob risco de perder a própria sessão SSH.
- *Port-knocking* (via `knockd`) mantém uma porta fechada até que uma sequência correta de "batidas" em outras portas a libere — testável com `nmap` e `ssh`.
- Trocar portas padrão (SSH 22, knock 7000/8000/9000) dificulta ataques automatizados.

## Relação com Outras Aulas

Esta aula fecha a progressão do curso reunindo conhecimentos das duas anteriores: os comandos de manipulação de arquivos da **Aula 01** são reaplicados nas rotinas de backup com `cron`; o `nmap`, também introduzido na **Aula 01**, passa a ser usado como ferramenta de teste e validação de firewall; e o editor `vi`, apresentado na **Aula 02**, é a ferramenta usada para editar os arquivos de configuração do `iptables` e do `knockd` explorados aqui. As recomendações de segurança para SSH da **Aula 02** são implementadas tecnicamente nesta aula através de `iptables` e *port-knocking*.
