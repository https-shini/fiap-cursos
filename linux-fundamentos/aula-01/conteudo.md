# Aula 01 — Introdução e Primeiros Comandos

[← Voltar ao índice do curso](../README.md)

## Objetivos de Aprendizagem

Introduzir o aluno ao terminal de comandos do Linux, justificando sua relevância mesmo em um mundo dominado por interfaces gráficas, e apresentar o conjunto de comandos essenciais para operar o sistema: comandos administrativos básicos, gerenciamento de usuários, manipulação de arquivos e diretórios, e configuração de rede.

## Conceitos Fundamentais

- Por que o terminal de comandos continua relevante na administração de sistemas
- Diferenças entre distribuições Linux e terminais (shells)
- Estrutura do prompt de comandos e case-sensitivity
- Comandos de ajuda, troca de usuário e gerenciamento de pacotes
- Gerenciamento de usuários e grupos
- A árvore de diretórios do Linux e comandos de manipulação de arquivos
- Compactação, download e configuração básica de rede

## Conteúdo

### 1. Por que aprender comandos em um mundo de interfaces gráficas?

Embora o uso de interfaces gráficas (GUIs) seja predominante no dia a dia, o terminal de comandos continua sendo indispensável por diversas razões:

- **Eficiência**: uma única linha de comando, com os parâmetros corretos, pode substituir a navegação por múltiplas telas de configuração em uma interface gráfica (o material usa como exemplo as opções do Microsoft Word, que exigiriam navegar por várias seções, contra um único comando parametrizado).
- **Leveza e velocidade**: o terminal é a forma mais rápida e leve de interagir com o kernel de um sistema operacional. Não exige placa gráfica potente, muita memória RAM ou sequer um monitor colorido.
- **Realidade dos servidores**: a maioria das implementações de servidores Linux **não conta com interface gráfica**, pois a instalação de um servidor X e um gerenciador de janelas representaria desperdício de recursos.
- **Acesso remoto eficiente**: o terminal viabiliza o acesso remoto via SSH, cujo tráfego de dados é infinitamente menor do que ferramentas gráficas como o TeamViewer, permitindo administração remota mesmo com links de dados limitados.

> Mesmo o Windows nunca abandonou seu terminal de comandos (o antigo MS-DOS evoluiu para o **PowerShell**), reforçando que a linha de comando é uma ferramenta transversal na administração de sistemas.

O Linux também conta com gerenciadores de janelas robustos (KDE, Gnome, XFCE, Window Maker, entre outros), mas o domínio do terminal continua sendo a habilidade fundamental para qualquer profissional de TI.

### 2. Considerações antes de começar

**Distribuições Linux**: existem centenas de distribuições (Red Hat, Fedora, Debian, Ubuntu, Slackware, Gentoo, Arch Linux etc.), algumas "aparentadas" entre si — por exemplo, o Ubuntu é considerado "filho" do Debian. O material adota a distribuição **Debian** como referência, por ser a "distribuição mãe" de muitas outras e por se manter gratuita e livre.

**Terminais (shells)**: existem diversos terminais de comando no Linux — `bash`, `csh`, `zsh`, entre outros. O curso utiliza o **bash** (*Bourne-Again Shell*), evolução livre do shell tradicional do Unix (`sh`), por ser o mais universalmente disponível: mesmo usuários que preferem outros shells costumam manter o bash instalado.

**O prompt de comandos**: é composto, geralmente, por:

```
usuário@hostname:diretório$
```

- `$` indica um usuário comum.
- `#` indica que o usuário logado é o **superusuário (root)**.

**Case-sensitivity**: o terminal Linux diferencia maiúsculas de minúsculas. O comando `ls` existe; o comando `LS` não.

Para fins didáticos, o material padroniza `$` para usuário comum e `#` para root ao longo dos exemplos.

### 3. Comandos essenciais

#### 3.1 `man` e `--help` — obtendo ajuda

O comando mais essencial de todos ensina uma lição fundamental: **não sabe, peça ajuda**.

```bash
man ls
```

Exibe o manual completo do comando: descrição, sinopse, parâmetros e, em alguns casos, exemplos de uso. O manual pode aparecer em português caso o pacote de idioma esteja instalado.

- Navegação: setas para cima/baixo.
- Para sair: tecla `q` (*quit*).

Alternativa mais rápida (resumo do manual, sem travar o terminal):

```bash
ls --help
```
Navegação com `[CTRL]+[PAGE UP]` / `[CTRL]+[PAGE DOWN]`.

#### 3.2 `su` — tornando-se outro usuário

Por segurança, recomenda-se operar sempre com um usuário de permissões restritas, elevando privilégios apenas quando necessário.

```bash
su            # torna-se root (padrão quando nenhum usuário é especificado)
su outrousuario   # torna-se um usuário específico
```

- Solicita a senha do usuário-alvo.
- Se executado a partir do **root**, a senha do usuário-alvo não é solicitada — "o todo-poderoso root" pode se tornar qualquer usuário livremente.
- O comando `whoami` ("quem sou eu?") ajuda a confirmar qual usuário está ativo quando o prompt não deixa isso explícito.

#### 3.3 `apt` — gerenciamento de pacotes (sistemas .deb)

O `apt` gerencia pacotes de software em sistemas baseados em `.deb` (Debian e derivados). **Não funciona** em sistemas `.rpm` (Red Hat, Fedora, CentOS, openSUSE) nem em outros como Slackware ou Gentoo.

| Comando | Função |
|---|---|
| `apt update` | Atualiza a referência dos repositórios (baixa a lista de pacotes disponíveis e suas versões) |
| `apt upgrade` | Atualiza os pacotes desatualizados, mediante confirmação |
| `apt search <termo>` | Busca por pacotes disponíveis antes de instalá-los |
| `apt install <pacote>` | Instala um pacote (requer privilégios de root) |
| `apt remove <pacote>` | Desinstala um pacote |

```bash
apt update
apt upgrade
apt install sudo
```

> **Observação**: o pacote `sudo` não vem instalado por padrão no Debian e deve ser instalado manualmente com o comando acima.

#### 3.4 `sudo` — executando comandos como superusuário

Permanecer como root o tempo todo é arriscado. O `sudo` permite elevar privilégios **pontualmente**, apenas para o comando específico.

**Configuração inicial (como root):**
```bash
apt install sudo
usermod -aG sudo hpoyatos    # adiciona o usuário ao grupo sudo
```

> É necessário deslogar e logar novamente após adicionar o usuário ao grupo, pois a alocação de grupos ocorre no início da sessão.
> Em algumas distribuições, o grupo equivalente se chama `wheel`. Pode-se conferir com `cat /etc/sudoers`.

**Uso:**
```bash
sudo apt install nmap
```
O terminal solicita a senha do **próprio usuário** (não a do root) e, se ele for membro do grupo `sudo`, executa o comando com privilégios elevados.

> **Boa prática**: preferir `sudo` a `su`, pois os privilégios são concedidos pontualmente, reduzindo o risco de operar acidentalmente como root por tempo prolongado.

### 4. Gerenciamento de usuários

#### 4.1 `passwd` — modificar senha

```bash
passwd                 # altera a própria senha (solicita senha atual + nova + confirmação)
sudo passwd root       # como root, altera a senha de outro usuário sem pedir a senha atual
```

> O terminal Linux **não exibe** a senha digitada, nem mesmo com máscaras como `****`. Isso é comportamento normal, não uma falha.

#### 4.2 `useradd` — adicionar usuário

```bash
useradd -d /home/novousuario -p senha novousuario
```

- `-d`: define o diretório pessoal do usuário.
- `-p`: define a senha (alternativamente, pode-se usar `passwd` na sequência).
- Por padrão, cria automaticamente um grupo de mesmo nome do usuário e o inclui nele.
- Consultar `man useradd` para parâmetros adicionais (usuários temporários, associação prévia a grupos, etc.).

#### 4.3 `groupadd` — adicionar grupo

```bash
sudo groupadd nomedogrupo
```

#### 4.4 `usermod` — modificar usuário

Embora seja tecnicamente possível editar diretamente os arquivos `/etc/passwd` (configurações de usuário), `/etc/shadow` (senhas) e `/etc/group` (configurações de grupo), **essa prática não é recomendada**, pois qualquer erro pode comprometer o acesso dos usuários ao sistema.

| Parâmetro | Função |
|---|---|
| `-aG <grupo>` | Adiciona o usuário a um novo grupo, **sem removê-lo** dos grupos atuais |
| `-d <diretório>` | Define novo diretório pessoal |
| `-L` | Bloqueia o usuário (*lock*) |
| `-U` | Desbloqueia o usuário (*unlock*) |
| `-e <data>` | Define data de expiração da conta |

```bash
sudo usermod -aG sudo,fiapon -e 2018-12-31 fiap
```

> **Atenção**: usar sempre `-aG` (append + group) e não apenas `-G`, pois este último **substitui** todos os grupos do usuário pelos informados, removendo-o dos demais.

### 5. Manipulação de arquivos e pastas

#### 5.1 A raiz de diretórios do Linux

Diferente do Windows — onde cada volume recebe uma letra (`C:\`, `D:\`) e possui sua própria raiz —, sistemas `*nix` possuem **uma única raiz** (`/`), na qual todos os demais volumes são "pendurados" (montados).

**Quadro 1.1 — Diretórios em um sistema Linux:**

| Diretório | Descrição | Equivalente aproximado no Windows |
|---|---|---|
| `/` | Diretório-raiz; ponto de partida de toda a árvore | — (Windows tem uma raiz por volume) |
| `/bin` | Binários (comandos) executáveis por qualquer usuário | — |
| `/boot` | Arquivos necessários para o boot do sistema | — |
| `/dev` | Dispositivos (mouses, impressoras, HDs, partições) representados como arquivos | Gerenciador de Dispositivos |
| `/etc` | Arquivos de configuração do sistema e serviços | — |
| `/home` | Diretórios pessoais dos usuários | `C:\Usuários` / `C:\Users` |
| `/lib` e `/lib64` | Bibliotecas de sistema (32 bits e 64 bits, respectivamente) | `C:\Windows\System` / `System32` |
| `/lost+found` | "Achados e perdidos"; arquivos recuperados pelo `fsck` após falhas | — |
| `/media` e `/mnt` | Pontos de montagem de volumes (pendrives, CDs, DVDs) | — |
| `/opt` | Diretório opcional para instalação alternativa de programas | — |
| `/proc` | Diretório virtual com informações do sistema (processador, memória, etc.) | — |
| `/root` | Diretório pessoal do superusuário | — |
| `/run` | Arquivos `.pid` de processos em execução | — |
| `/sbin` | Binários de uso exclusivo do superusuário | — |
| `/srv` | Dados de serviços em execução | — |
| `/sys` | Módulos para equipamentos USB (kernels 2.6.x+) | — |
| `/tmp` | Arquivos temporários | `C:\Windows\Temp` |
| `/usr` | Onde os programas são instalados | `C:\Arquivos de programas` |
| `/var` | Dados de tamanho variável (ex.: bancos de dados) | — |
| `/var/log` | Logs do sistema | — |

#### 5.2 `ls` — listar arquivos e diretórios

```bash
ls              # listagem simples (verde = arquivo, azul = diretório)
ls -l -a        # -l: lista longa/detalhada; -a: exibe também arquivos ocultos (iniciados com ".")
ls -lah /       # -h: tamanhos legíveis para humanos (KB, MB, GB); aplicado ao diretório raiz
```

> **Atenção**: extensões de arquivo no Linux são **dispensáveis** e não definem o tipo do arquivo (diferente do Windows).

**Estrutura da saída de `ls -la`:**

| Coluna | Conteúdo |
|---|---|
| 1ª | Tipo de arquivo e permissões (10 caracteres: tipo + 3x3 permissões `rwx` para dono/grupo/outros) |
| 2ª | Quantidade de arquivos/subitens (arquivos sempre "1"; diretórios variam) |
| 3ª | Dono do arquivo (nome ou UID) |
| 4ª | Grupo dono do arquivo (nome ou GID) |
| 5ª | Tamanho em bytes (ou legível com `-h`) |
| 6ª | Data da última modificação |
| 7ª | Nome do arquivo/diretório |

Tipos de arquivo na 1ª coluna: `-` (arquivo comum), `d` (diretório), `l` (link), `b`/`c` (dispositivos de bloco/caractere).
Permissões: `r` (read/leitura), `w` (write/escrita), `x` (execute/execução).

#### 5.3 `pwd` — mostrar diretório atual

```bash
pwd     # print work directory
```
Útil quando o prompt não exibe o diretório completo (o símbolo `~` sempre representa o diretório pessoal do usuário atual).

#### 5.4 `cd` — mudar de diretório

```bash
cd /home/hpoyatos     # caminho absoluto
cd hpoyatos            # caminho relativo (a partir do diretório atual)
cd ..                  # sobe um nível (diretório pai)
cd                     # sem argumento: retorna ao diretório-padrão do usuário (equivale a cd ~)
```

#### 5.5 `mkdir` — criar diretório

```bash
mkdir "Nova Pasta"
mkdir -p novapasta/subpasta/subsubpasta
```

- Nomes com espaços ou caracteres especiais são permitidos, desde que "escapados" com barra invertida ou aspas.
- O parâmetro `-p` cria toda a hierarquia de subdiretórios de uma vez; sem ele, o comando falha se os diretórios intermediários não existirem.
- O sistema **não confirma sucesso** — mensagens só aparecem em caso de erro.

#### 5.6 `rm` — apagar arquivo ou diretório

```bash
rm /home/hpoyatos/Arquivo1.txt          # apaga um arquivo
rm /home/hpoyatos/pasta/subpasta        # apaga diretório VAZIO
rm -rf /home/hpoyatos/NovaPasta2        # apaga diretório e todo o conteúdo, sem confirmação
```

| Parâmetro | Função |
|---|---|
| `-r` | Recursivo — remove subdiretórios e seus conteúdos em cascata |
| `-f` | Força — não pede confirmação a cada item (*force*) |

> ⚠️ **Extremo cuidado**: o comando `rm -rf /`, executado como root, apagaria todo o sistema de arquivos. Não existe "Lixeira" em terminal — **não há como desfazer**. O comando só afeta arquivos para os quais o usuário tem permissão, exceto quando executado como root.

#### 5.7 `cp` — copiar arquivos e diretórios

```bash
cp /home/hpoyatos/Arquivo2.txt /tmp                          # copia mantendo o nome
cp /home/hpoyatos/Arquivo2.txt /tmp/NovoNome2.txt             # copia e renomeia
cp -Rvp /home/hpoyatos/Imagens /tmp                            # copia diretório inteiro
```

| Parâmetro | Função |
|---|---|
| `-R` | Recursivo — copia o diretório e todo o conteúdo |
| `-v` | *Verbose* — exibe os arquivos copiados |
| `-p` | Preserva as permissões originais |

> É necessário ter permissão de escrita no diretório de destino.

#### 5.8 `mv` — mover ou renomear

```bash
mv /tmp/Imagens /opt                       # move um diretório
mv /tmp/Arquivo2.txt /tmp/Arquivo1.txt     # renomeia um arquivo
```

#### 5.9 `cat` — exibir conteúdo de um arquivo

```bash
cat /proc/locks
```

#### 5.10 `head` — exibir linhas iniciais

```bash
head -n 10 /proc/cpuinfo
```
Útil para inspecionar arquivos grandes sem carregar tudo.

#### 5.11 `tail` — exibir linhas finais

```bash
sudo tail -n 10 /var/log/messages
```
Muito utilizado para leitura de logs, que costumam ser extensos e cujo conteúdo mais relevante geralmente está no final.

#### 5.12 `less` — visualizar arquivo com navegação

```bash
sudo less /var/log/messages
```
Permite navegar com as setas (diferente do `cat`, que exige `[CTRL]+[PAGE UP/DOWN]`). Para sair, digite `q`.

#### 5.13 `locate` — localizar arquivos

```bash
sudo apt install locate
sudo updatedb          # atualiza a base de dados de busca (executar periodicamente)
locate Imagem1.png
```

#### 5.14 `grep` — buscar termos dentro de arquivos

```bash
grep fiap /var/log/auth.log                 # busca em um único arquivo
sudo grep -r "FIAP ON" /home                 # -r: busca recursiva em um diretório inteiro
```

#### 5.15 `unzip` — descompactar arquivos .zip

```bash
unzip -v arquivoZipado.zip     # -v: lista os arquivos descompactados
unzip -tv arquivoZipado.zip    # -t: testa a integridade sem descompactar
```

#### 5.16 `tar` — (des)empacotar e (des)comprimir arquivos

O `tar` **empacota** (une vários arquivos em um só) e pode, opcionalmente, **comprimir** o pacote usando outro utilitário integrado (`gzip` ou `bzip2`).

```bash
tar -czvf exemplo1.tar.gz /home/hpoyatos           # empacota e comprime tudo (recursivo)
tar -czvf exemplo2.tgz *.txt                        # filtra apenas arquivos .txt
tar -xzvf exemplo1.tar.gz                            # extrai (des-empacota e descomprime)
tar -tzvf exemplo1.tar.gz                            # testa/lista o conteúdo sem extrair
```

| Parâmetro | Função |
|---|---|
| `-c` | Cria o pacote (*create*) |
| `-x` | Extrai o pacote (*extract*) |
| `-t` | Testa/lista o conteúdo sem extrair |
| `-z` | Comprime/descomprime com `gzip` |
| `-j` | Comprime/descomprime com `bzip2` (mais eficaz, porém mais lento) |
| `-v` | *Verbose* |
| `-f` | Define o nome do arquivo de destino/origem |

> Convenções de extensão: `.tar.gz` ou `.tgz` para gzip; `.tar.bz2` ou `.tbz` para bzip2. As extensões são opcionais, mas ajudam a identificar como descompactar o arquivo posteriormente.

#### 5.17 `wget` — baixar arquivos da internet

```bash
wget https://exemplo.com/arquivo.tar.xz
wget -c https://exemplo.com/arquivo.tar.xz     # -c: continua (retoma) um download interrompido
```

> Um navegador web em modo texto (`links`) pode ser instalado com `sudo apt-get install links`, dispensando interface gráfica até mesmo para localizar endereços web.

### 6. Configuração de rede

Pré-requisito: instalar o pacote `net-tools`.

```bash
sudo apt install net-tools
```

#### 6.1 `ifconfig` — configuração de interfaces de rede

```bash
sudo ifconfig                                            # exibe todas as interfaces
sudo ifconfig enp0s3 down | up                            # desliga/liga uma interface
sudo ifconfig enp0s3 172.16.0.10 netmask 255.255.0.0      # altera IP e máscara
```

- Interfaces de rede eram tradicionalmente nomeadas `eth0`, `eth1`, etc., mas hoje variam conforme o driver.
- A interface `lo` (*loopback*) sempre existe, com IP padrão `127.0.0.1`, permitindo que a máquina se comunique consigo mesma mesmo com a interface de rede real inativa.

#### 6.2 `route` — gerenciamento de rotas de rede

```bash
sudo route                                                                    # exibe rotas atuais
sudo route add -net 192.36.73.0 netmask 255.255.255.0 dev enp0s3             # adiciona rota para uma rede
sudo route add default gw 10.0.2.1                                            # define gateway-padrão
```

Útil em servidores com múltiplas interfaces conectadas a sub-redes diferentes, permitindo otimizações estratégicas de infraestrutura.

#### 6.3 `nmap` — escaneamento de portas

```bash
sudo apt install nmap
nmap 127.0.0.1                          # escaneamento padrão
nmap --all-ports -sV 127.0.0.1          # escaneia todas as portas e identifica serviço/versão
```

| Parâmetro | Função |
|---|---|
| `--all-ports` | Escaneia todas as portas (o padrão ignora algumas) |
| `-s` | Informa qual serviço está escutando |
| `-V` | Informa a versão do serviço |

> **Atenção ética e legal**: escaneamento de portas deve ser realizado **apenas em máquinas próprias ou com autorização**, pois a prática é considerada um ato de reconhecimento preliminar a uma invasão.
>
> Informações obtidas via nmap (como a versão exata do OpenSSH) podem ser usadas por um atacante para identificar vulnerabilidades conhecidas (*exploits*) em versões desatualizadas. Por isso, **desligar serviços não utilizados** e manter softwares atualizados são medidas importantes de segurança.

## Exemplos

| Cenário | Comando |
|---|---|
| Consultar o manual de um comando | `man ls` |
| Tornar-se root | `su` |
| Atualizar repositórios e pacotes | `sudo apt update && sudo apt upgrade` |
| Adicionar usuário ao grupo sudo | `sudo usermod -aG sudo hpoyatos` |
| Criar usuário e grupo | `sudo useradd -d /home/novo novo` / `sudo groupadd equipe` |
| Listar arquivos ocultos com detalhes | `ls -lah` |
| Criar estrutura de diretórios | `mkdir -p a/b/c` |
| Apagar diretório inteiro sem confirmação | `sudo rm -rf pasta` |
| Copiar diretório preservando permissões | `cp -Rvp origem destino` |
| Buscar um termo recursivamente | `sudo grep -r "termo" /home` |
| Empacotar e comprimir um diretório | `tar -czvf backup.tar.gz /home/usuario` |
| Baixar arquivo retomando de onde parou | `wget -c URL` |
| Alterar IP de uma interface | `sudo ifconfig enp0s3 IP netmask MASCARA` |
| Escanear portas com detalhamento | `sudo nmap --all-ports -sV IP` |

## Aplicações Práticas

### Cheat sheet de comandos

```bash
# Ajuda
man <comando>
<comando> --help

# Usuários e privilégios
su [usuario]
sudo <comando>
passwd [usuario]
useradd -d <dir> -p <senha> <usuario>
groupadd <grupo>
usermod -aG <grupo> <usuario>

# Pacotes (Debian/.deb)
apt update
apt upgrade
apt search <termo>
apt install <pacote>
apt remove <pacote>

# Arquivos e diretórios
ls -lah [caminho]
pwd
cd [caminho]
mkdir -p <caminho>
rm -rf <caminho>
cp -Rvp <origem> <destino>
mv <origem> <destino>
cat <arquivo>
head -n <N> <arquivo>
tail -n <N> <arquivo>
less <arquivo>
locate <arquivo>
grep -r "<termo>" <diretorio>
unzip -tv <arquivo.zip>
tar -czvf <destino.tar.gz> <origem>
tar -xzvf <arquivo.tar.gz>
wget -c <URL>

# Rede
ifconfig
ifconfig <interface> up|down
ifconfig <interface> <IP> netmask <MASCARA>
route add -net <REDE> netmask <MASCARA> dev <interface>
route add default gw <GATEWAY>
nmap --all-ports -sV <IP>
```

## Boas Práticas

- Preferir `sudo` a `su` para operações administrativas pontuais, minimizando o tempo em que o usuário opera com privilégios totais.
- Sempre usar `-aG` (não `-G`) com `usermod` para adicionar usuários a grupos sem removê-los dos grupos já existentes.
- Ter extremo cuidado com `rm -rf`, especialmente como root: não há "lixeira" de recuperação no terminal.
- Preferir caminhos absolutos em comandos destrutivos (`rm`, `mv`) para reduzir o risco de operar no diretório errado.
- Manter serviços de rede atualizados e desligar os que não são utilizados, reduzindo a superfície de ataque identificável via `nmap`.
- Realizar escaneamento de portas apenas em ambientes próprios ou autorizados.
- Lembrar que o Linux é case-sensitive em todos os comandos, argumentos e nomes de arquivos.

## Resumo

Esta aula introdutória apresentou a justificativa para o domínio do terminal Linux, mesmo em cenários com interface gráfica disponível, e forneceu a base de comandos essenciais para operação do sistema: obtenção de ajuda (`man`, `--help`), troca e elevação de privilégios (`su`, `sudo`), gerenciamento de pacotes (`apt`), gerenciamento de usuários e grupos (`passwd`, `useradd`, `groupadd`, `usermod`), navegação e manipulação do sistema de arquivos (`ls`, `pwd`, `cd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `head`, `tail`, `less`, `locate`, `grep`), compactação e transferência de arquivos (`unzip`, `tar`, `wget`) e configuração básica de rede (`ifconfig`, `route`, `nmap`). Esses comandos formam a base sobre a qual as aulas seguintes constroem tópicos mais avançados de administração.

---

## Principais Pontos a Memorizar

- O terminal é a forma mais leve e rápida de interagir com o kernel — indispensável em servidores sem interface gráfica e em acesso remoto via SSH.
- O curso usa **Debian** como distribuição de referência e **bash** como shell padrão; o terminal Linux é *case-sensitive*.
- `$` no prompt indica usuário comum; `#` indica root.
- `man <comando>` e `<comando> --help` são o primeiro recurso diante de qualquer dúvida.
- `apt` gerencia pacotes apenas em sistemas `.deb` (Debian/derivados); não funciona em `.rpm`.
- `sudo` executa um único comando com privilégios de root sem trocar de sessão; `su` troca de usuário efetivamente.
- Estrutura básica de diretórios: `/bin` (binários essenciais), `/` (raiz), `/home` (diretórios pessoais dos usuários) — `/boot`, não `/`, guarda os arquivos de boot.
- Comandos essenciais de arquivos: `ls`, `pwd`, `cd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `head`, `tail`, `less`, `locate`, `grep`, `tar`, `unzip`, `wget`.
- `ifconfig` configura interfaces de rede; `nmap` escaneia portas — ferramenta que reaparece na Aula 03 para validar firewall.

## Relação com Outras Aulas

Os comandos essenciais desta aula (navegação, manipulação de arquivos e diretórios, `grep`, `tar`) são a base reutilizada em toda a trilha: a **Aula 03** os aplica diretamente na criação de rotinas de backup automatizadas com `cron`, e o `nmap`, apresentado aqui como ferramenta de escaneamento de portas, reaparece na **Aula 03** para validar regras de firewall e o funcionamento do *port-knocking*. O domínio de `sudo` e do gerenciamento de usuários também é pré-requisito direto para as operações de superusuário exigidas nas Aulas 02 e 03.
