# Documentação Completa do Curso — Linux Fundamentos (FIAP)

> Documento consolidado a partir do material didático disponível no repositório [`fiap-cursos/Linux Fundamentos`](https://github.com/https-shini/fiap-cursos), contendo os três capítulos do curso: Introdução e primeiros comandos, Administração de sistema e editor de texto, e Recursos avançados.

## Sumário
- [Aula 01 — Introdução e Primeiros Comandos](#aula-01--introdução-e-primeiros-comandos)
- [Aula 02 — Administração de Sistema e Editor de Texto](#aula-02--administração-de-sistema-e-editor-de-texto)
- [Aula 03 — Recursos Avançados](#aula-03--recursos-avançados)
- [Visão Geral do Curso](#visão-geral-do-curso)

---

# Aula 01 — Introdução e Primeiros Comandos

**Disciplina:** Linux Fundamentos
**Instituição:** FIAP ON

## Objetivos

Introduzir o aluno ao terminal de comandos do Linux, justificando sua relevância mesmo em um mundo dominado por interfaces gráficas, e apresentar o conjunto de comandos essenciais para operar o sistema: comandos administrativos básicos, gerenciamento de usuários, manipulação de arquivos e diretórios, e configuração de rede.

## Conceitos Principais

- Por que o terminal de comandos continua relevante na administração de sistemas
- Diferenças entre distribuições Linux e terminais (shells)
- Estrutura do prompt de comandos e case-sensitivity
- Comandos de ajuda, troca de usuário e gerenciamento de pacotes
- Gerenciamento de usuários e grupos
- A árvore de diretórios do Linux e comandos de manipulação de arquivos
- Compactação, download e configuração básica de rede

## Conteúdo da Aula

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

## Exemplos Práticos

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

## Comandos e Código

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

## Boas Práticas e Pontos de Atenção

- Preferir `sudo` a `su` para operações administrativas pontuais, minimizando o tempo em que o usuário opera com privilégios totais.
- Sempre usar `-aG` (não `-G`) com `usermod` para adicionar usuários a grupos sem removê-los dos grupos já existentes.
- Ter extremo cuidado com `rm -rf`, especialmente como root: não há "lixeira" de recuperação no terminal.
- Preferir caminhos absolutos em comandos destrutivos (`rm`, `mv`) para reduzir o risco de operar no diretório errado.
- Manter serviços de rede atualizados e desligar os que não são utilizados, reduzindo a superfície de ataque identificável via `nmap`.
- Realizar escaneamento de portas apenas em ambientes próprios ou autorizados.
- Lembrar que o Linux é case-sensitive em todos os comandos, argumentos e nomes de arquivos.

## Resumo da Aula

Esta aula introdutória apresentou a justificativa para o domínio do terminal Linux, mesmo em cenários com interface gráfica disponível, e forneceu a base de comandos essenciais para operação do sistema: obtenção de ajuda (`man`, `--help`), troca e elevação de privilégios (`su`, `sudo`), gerenciamento de pacotes (`apt`), gerenciamento de usuários e grupos (`passwd`, `useradd`, `groupadd`, `usermod`), navegação e manipulação do sistema de arquivos (`ls`, `pwd`, `cd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `head`, `tail`, `less`, `locate`, `grep`), compactação e transferência de arquivos (`unzip`, `tar`, `wget`) e configuração básica de rede (`ifconfig`, `route`, `nmap`). Esses comandos formam a base sobre a qual as aulas seguintes constroem tópicos mais avançados de administração.

---

# Aula 02 — Administração de Sistema e Editor de Texto

**Disciplina:** Linux Fundamentos
**Autor do material:** Filipi Pires
**Instituição:** FIAP ON

## Objetivos

Aprofundar o conhecimento em administração de sistemas Linux por meio do gerenciamento de processos, do acesso remoto seguro via SSH e do domínio do editor de texto em linha de comando **vi**, ferramenta essencial para edição de arquivos de configuração em servidores sem interface gráfica.

## Conceitos Principais

- Processos: o que são, como monitorá-los e como encerrá-los
- Execução de comandos em primeiro e segundo plano (*foreground*/*background*)
- Terminais virtuais persistentes com `screen`
- Acesso remoto seguro com SSH e transferência de arquivos com SCP
- Reforço de segurança do serviço SSH
- Fundamentos do editor de texto **vi**: modos de operação, edição, navegação, cópia/colagem, busca e substituição

## Conteúdo da Aula

### 1. Gerenciamento de Processos

Um programa é um arquivo inerte armazenado em disco. Ao ser executado (via comando ou clique em um lançador gráfico), ele se torna um **processo** — um programa em execução. Processos podem travar ou consumir recursos excessivos, tornando essencial saber monitorá-los e, se necessário, encerrá-los.

#### 1.1 `ps` — listar processos

```bash
sudo ps -au
```

| Parâmetro | Função |
|---|---|
| `-a` | Mostra processos de todos os usuários (requer root) |
| `-u` | Exibe lista detalhada, incluindo o usuário dono do processo |
| `-x` | Exibe processos não associados a um terminal de comandos (ex.: processos da interface gráfica) |

> A combinação `-x` costuma tornar a listagem muito extensa; por isso, o material recomenda `ps -au` para uma visão equilibrada.

#### 1.2 `top` — monitor de processos em tempo real

O `top` vai além de listar processos: exibe o total de processos em andamento, seus estados (executando, em espera, parados, *zombies*), e o uso de CPU, memória RAM e memória de *swap*, atualizado em tempo real. Processos que mais consomem recursos aparecem no topo da lista (tipicamente processos de interface gráfica, como `gnome-shell` e `Xorg`).

```bash
top
```
Para sair (o comando trava o terminal): tecla `q`.

#### 1.3 `kill` e `killall` — encerrando processos

Para "matar" um processo, primeiro é preciso identificar seu **PID** (*Process ID*), obtido via `ps`. O símbolo `|` (*pipe*) permite encadear comandos, combinando `ps` com `grep` para filtrar resultados:

```bash
ps aux | grep gnome-chess
```

Com o PID identificado (ex.: 2829):

```bash
kill 2829
```

> Não é necessário `sudo` quando o processo pertence ao próprio usuário que executa o comando.

**Sinais além da destruição:** embora o nome sugira apenas "matar", o `kill` pode enviar diferentes sinais ao processo:

```bash
kill -SIGSTOP 2829     # pausa/congela o processo, sem destruí-lo
kill -SIGCONT 2829     # retoma o processamento de onde parou
```

> A omissão do parâmetro de sinal equivale a `SIGKILL`, que efetivamente destrói o processo. O `SIGSTOP` é útil, por exemplo, para pausar temporariamente um comando `tar` demorado sem perder o progresso, liberando recursos do sistema momentaneamente.

**`killall`** — variação que encerra **todos os processos com o mesmo nome** (útil para serviços escaláveis, como o Apache, que gera múltiplos subprocessos):

```bash
sudo /etc/init.d/apache2 start      # inicia o daemon do Apache
ps aux | grep apache2                # revela processo principal + subprocessos
sudo killall apache2                 # encerra todos de uma vez
```

> `sudo` é necessário aqui porque o serviço pertence ao usuário `www-data`, diferente do usuário que está executando o comando.

Outra forma de interromper processos é simplesmente **fechar o terminal** ao qual eles estão associados, já que todo processo possui um terminal de origem (gráfico ou não).

#### 1.4 `&` — executando processos em segundo plano (*background*)

Por padrão, todo comando "trava" o terminal até sua conclusão, quando então devolve o *prompt*. Esse comportamento é evidente em aplicativos gráficos executados a partir do terminal, como o `gnome-chess`:

```bash
gnome-chess          # trava o terminal (executando em foreground)
gnome-chess &        # devolve o prompt imediatamente, mostrando o PID (background)
```

> Rodar em segundo plano **não desvincula** o processo do terminal: se o terminal for fechado, o processo (e a aplicação) é encerrado junto.

#### 1.5 `screen` — criando terminais virtuais persistentes

Processos sempre estarão vinculados a um terminal — mas o `screen` permite criar **terminais virtuais** que podem ser abandonados e retomados posteriormente, mesmo remotamente. É especialmente útil para:

- Comandos de longa duração (backups com `tar`, downloads grandes com `wget`).
- Iniciar um processo fisicamente no servidor e verificar seu progresso depois, remotamente.

```bash
sudo apt install screen
screen -S terminal3          # cria um terminal virtual nomeado
# [CTRL]+[A], depois [D]     # abandona (detach) o terminal sem encerrá-lo
screen -ls                    # lista os terminais virtuais existentes
screen -r terminal2           # retoma (reattach) um terminal específico
exit                          # encerra a execução dentro do terminal virtual
```

### 2. Acesso a Máquinas Remotas com SSH

O acesso remoto é uma necessidade central na computação moderna (Cloud Computing), onde frequentemente não se sabe fisicamente onde está o servidor sendo operado. Historicamente, "terminais burros" (apenas monitor e teclado, sem processamento) atendiam a essa necessidade.

#### 2.1 De telnet a SSH

O **telnet** foi o serviço pioneiro para acesso remoto no Unix (permitindo até terminal via linha discada), mas tornou-se obsoleto por falta de segurança: os comandos trafegam **sem criptografia**, tornando a comunicação vulnerável a **session hijacking** (o atacante sequestra a sessão, derrubando o cliente legítimo e se passando por ele).

O **SSH** (*Secure Shell*) é a evolução segura do telnet: mantém toda a praticidade do acesso remoto, mas trafega as informações através de um **túnel criptografado**.

#### 2.2 `ssh` — acesso remoto

```bash
ssh usuario@host              # host pode ser IP ou hostname
ssh usuario@host -p <porta>   # se o SSH não estiver na porta padrão (22)
```

**Criptografia assimétrica (chave público-privada):** cada host gera um par de chaves — uma pública (distribuída) e uma privada (mantida em sigilo). As informações trafegam usando a chave privada de origem e a pública do destino, sendo decifradas apenas com a combinação inversa (chave pública de origem + chave privada do destino), tornando o acesso simultaneamente prático e seguro.

No primeiro acesso a um host, o SSH apresenta sua **fingerprint** (chave pública) para confirmação:

```
The authenticity of host '192.168.0.100' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no)?
```

Uma vez conectado, todos os comandos aprendidos no curso funcionam normalmente, como se o usuário estivesse fisicamente diante do equipamento. Para retornar ao host de origem:

```bash
exit
```

**Facilitando o acesso com `/etc/hosts`:** mesmo com um servidor DNS configurado, o sistema sempre consulta `/etc/hosts` primeiro. É possível adicionar uma entrada manualmente (como root, pois o arquivo é protegido):

```bash
echo "192.168.0.100 raspberrypi" >> /etc/hosts
```

> ⚠️ **Atenção crítica**: usar sempre `>>` (acrescenta ao final do arquivo) e nunca `>` (sobrescreve e **apaga todo o conteúdo** de `/etc/hosts`).

Após o cadastro, o acesso pode ser feito diretamente pelo *hostname*:

```bash
ssh usuario@raspberrypi
```

#### 2.3 `scp` — transferência segura de arquivos

O SSH também viabiliza a transferência de arquivos pelo mesmo canal criptografado, através do comando **SCP** (*Secure Copy*), dispensando o uso do FTP tradicional.

```bash
scp [origem] [destino]
```
A sintaxe usa o arquivo local como no comando `cp` e o destino/origem remoto como no `ssh`.

```bash
# Do local para o servidor remoto:
scp Imagens/Imagem1.png pi@raspberrypi:/tmp

# Do servidor remoto para o local:
scp pi@raspberrypi:/tmp/Imagem1.png .
```
> O ponto (`.`) representa o diretório atual.

#### 2.4 Subindo um serviço SSH

Para disponibilizar acesso SSH em uma máquina:

```bash
sudo apt install ssh
sudo /etc/init.d/ssh start
```

#### 2.5 Tornando o SSH ainda mais seguro

Embora já conte com criptografia e configuração inicial razoavelmente segura, o SSH é um dos serviços mais visados por invasores devido ao seu potencial de controle remoto. Recomendações práticas:

- **Use senhas seguras**: o ataque mais comum é o de **força bruta** (*brute force attack*), no qual o atacante testa combinações sistematicamente; um **ataque de dicionário** automatiza esse processo usando palavras comuns de um idioma. Senhas simples são descobertas rapidamente.
- **Configure o serviço** (`/etc/ssh/sshd_config`): limite o número de tentativas de senha, impeça a autenticação direta do usuário `root`, restrinja o serviço às interfaces de rede necessárias e, se possível, restrinja o acesso a hosts conhecidos (cujas chaves públicas já foram cadastradas).
- **Restrinja quais usuários podem usar SSH**: no mesmo arquivo, use as diretivas:
  - `allowuser` — lista de usuários permitidos (separados por vírgula).
  - `allowgroup` — grupos permitidos.
  - `denyuser` / `denygroup` — usuários/grupos proibidos.
- **Troque a porta padrão** (22) por outro número no `sshd_config`.
- **Restrinja por IP**: usando `iptables` (tema de um capítulo posterior), é possível liberar apenas IPs específicos para a porta do SSH.
- **Implemente Port Knocking**: técnica avançada (também abordada em capítulo posterior) que libera a porta do serviço apenas quando pacotes de dados são enviados a portas específicas, em sequência determinada — podendo até fechar a porta automaticamente ao encerrar a sessão.

### 3. VI: o Editor de Texto em Linha de Comando

Embora existam outros editores (`emacs`, `nano`), o **vi** (ou **vim** — *Vi IMproved*) é apresentado como o editor mais poderoso e versátil para uso em terminal, sendo indispensável para editar arquivos de configuração em servidores remotos sem interface gráfica.

#### 3.1 Entrando e saindo do vi

```bash
vi arquivo.txt      # cria ou edita o arquivo
```

Para sair do editor: `[ESC]`, depois `:q` e `[ENTER]`.

> A tecla `[ESC]` sempre retorna ao **modo de comandos**, independentemente do modo em que o editor estava.

#### 3.2 Os dois modos do vi

O vi opera em dois modos fundamentais:

- **Modo de comandos**: as teclas digitadas disparam comandos do editor (copiar, apagar, navegar, salvar, etc.).
- **Modo de edição de texto** (*insert*): tudo o que é digitado se torna parte do documento.

**Formas de entrar em modo de edição a partir do modo de comandos:**

| Tecla | Efeito |
|---|---|
| `i` | Insere texto **no ponto exato** onde o cursor está |
| `a` | Insere texto **uma posição após** o cursor |
| `A` | Insere texto **no final da linha** atual |
| `o` | Insere texto **em uma nova linha**, logo abaixo da atual |

Em todos os casos, `[ESC]` retorna ao modo de comandos.

#### 3.3 Salvando o arquivo

Em **modo de comandos**:

| Sequência | Efeito |
|---|---|
| `:w` + `[ENTER]` | Salva (*write*) sem sair |
| `:wq` + `[ENTER]` | Salva e sai |
| `:x` + `[ENTER]` | Salva e sai (atalho preferido pelo autor, uma tecla a menos) |
| `:q!` + `[ENTER]` | Sai **sem salvar**, descartando alterações |

#### 3.4 Numeração de linhas e navegação direta

Para exibir a numeração das linhas (útil, por exemplo, ao localizar um erro apontado por um depurador em uma linha específica):

```
:set nu
```

Navegação direta para uma linha específica (em modo de comandos):

```
:11
```
(digite os dois pontos, o número da linha desejada e pressione `[ENTER]`)

> **Atenção**: navegar com as setas do teclado é seguro apenas em **modo de comandos**. Usá-las em modo de edição insere caracteres indesejados diretamente no documento.

#### 3.5 Copiando e colando textos

| Comando (modo de comandos) | Efeito |
|---|---|
| `yy` | Copia (*yank*) a linha inteira em que o cursor está |
| `y` (sozinho) | Copia um "parágrafo" (até encontrar uma linha em branco) |
| `10yy` | Copia a linha atual + as 9 seguintes (10 linhas no total) |
| `yw` | Copia uma única palavra (*word*), até o próximo espaço |
| `y$` | Copia do cursor até o **final** da linha |
| `y^` | Copia do cursor até o **início** da linha |
| `p` | Cola (*paste*) o conteúdo copiado |
| `G` | Vai para a **última linha** do arquivo |
| `:1` | Vai para a **primeira linha** do arquivo |

**Copiar um trecho para outro arquivo:**
```
:6,9w arquivo2.txt
```
Copia as linhas 6 a 9 do arquivo atual para um novo arquivo chamado `arquivo2.txt`.

> Os símbolos `^` (início de linha) e `$` (fim de linha) são notações clássicas de **expressões regulares**, úteis em diversas outras ferramentas Linux.

#### 3.6 Apagando textos

Os comandos de exclusão seguem a mesma lógica dos de cópia, substituindo `y` por `d` (*delete*):

| Comando | Efeito |
|---|---|
| `dd` | Apaga a linha atual (e a **recorta**, mantendo-a disponível para colar) |
| `10dd` | Apaga a linha atual + as 9 seguintes |
| `dw` | Apaga a palavra atual |
| `d$` | Apaga do cursor até o final da linha |
| `d^` | Apaga do cursor até o início da linha |
| `d→` | Apaga um único caractere |
| `u` + `[ENTER]` | Desfaz (*undo*) o **último** comando — apenas o último |

> Como `dd` na verdade **recorta** (não apenas apaga), seguir com `p` realiza um "recortar e colar" completo.

#### 3.7 Localizando e substituindo palavras

**Busca:**
```
/palavra-chave
[ENTER]
```
- `n` — avança para a próxima ocorrência.
- `N` — retorna para a ocorrência anterior.

**Substituição:**
```
:%s/mais/menos/gc
```

| Elemento | Função |
|---|---|
| `%s` | Aplica a substituição em todo o documento |
| `/mais/menos/` | Substitui "mais" por "menos" |
| `g` | *Global* — substitui **todas** as ocorrências na linha (sem ele, apenas a primeira de cada linha) |
| `c` | *Confirm* — pede confirmação antes de cada substituição (sem ele, troca tudo automaticamente) |

## Exemplos Práticos

| Cenário | Comando |
|---|---|
| Ver processos do usuário atual e detalhes | `ps -au` |
| Monitorar recursos em tempo real | `top` |
| Encerrar um processo pelo nome | `ps aux \| grep <nome>` seguido de `kill <PID>` |
| Encerrar todas as instâncias de um serviço | `sudo killall <serviço>` |
| Pausar/retomar um processo | `kill -SIGSTOP <PID>` / `kill -SIGCONT <PID>` |
| Rodar app gráfico sem travar o terminal | `<comando> &` |
| Iniciar backup longo e verificar depois remotamente | `screen -S backup`, `[CTRL]+[A]`+`[D]`, depois `screen -r backup` |
| Acessar servidor remoto | `ssh usuario@IP` |
| Copiar arquivo para servidor remoto | `scp arquivo.txt usuario@IP:/destino` |
| Cadastrar atalho de host | `echo "IP nome" >> /etc/hosts` |
| Criar/editar arquivo com vi | `vi arquivo.txt` |
| Salvar e sair do vi | `:x` |
| Substituir todas ocorrências no vi | `:%s/antigo/novo/gc` |

## Comandos e Código

```bash
# Processos
ps -au
top
ps aux | grep <nome>
kill <PID>
kill -SIGSTOP <PID>
kill -SIGCONT <PID>
sudo killall <nome>
<comando> &                 # roda em background

# Screen
sudo apt install screen
screen -S <nome>
screen -ls
screen -r <nome>

# SSH e SCP
sudo apt install ssh
sudo /etc/init.d/ssh start
ssh usuario@host [-p porta]
scp origem destino
echo "IP hostname" >> /etc/hosts

# vi
vi arquivo.txt
# Entrar em edição: i | a | A | o
# Sair da edição: [ESC]
# Salvar: :w   Salvar e sair: :wq  ou  :x   Sair sem salvar: :q!
# Numerar linhas: :set nu
# Ir para linha N: :N
# Copiar linha: yy   Colar: p
# Apagar linha: dd
# Desfazer: u
# Buscar: /termo   Próxima: n   Anterior: N
# Substituir: :%s/antigo/novo/gc
```

## Boas Práticas e Pontos de Atenção

- Preferir `sudo kill`/`killall` apenas quando o processo pertencer a outro usuário — se for o processo do próprio usuário, `sudo` é desnecessário.
- Usar `kill -SIGSTOP`/`-SIGCONT` para liberar recursos temporariamente em vez de encerrar (e perder) processos longos.
- Lembrar que processos em `&` (background) continuam vinculados ao terminal: fechar o terminal os encerra. Para independência real do terminal, usar `screen`.
- Sempre verificar a *fingerprint* apresentada no primeiro acesso SSH a um host.
- Usar `>>` (nunca `>`) ao editar arquivos de configuração como `/etc/hosts` via `echo`, para não apagar o conteúdo existente.
- Reforçar a segurança do SSH: senhas fortes, desabilitar login direto do root, restringir usuários/IPs, trocar a porta padrão.
- No vi, sempre confirmar em qual modo se está (comando ou edição) antes de navegar com as setas, para evitar poluir o documento com caracteres indesejados.
- Lembrar que `u` desfaz **apenas o último** comando no vi — não há histórico de múltiplos "undos" nesta versão básica apresentada.

## Resumo da Aula

A aula cobriu três frentes complementares da administração prática de sistemas Linux: o **gerenciamento de processos** (visualização com `ps`/`top`, encerramento com `kill`/`killall`, execução em segundo plano com `&` e terminais persistentes com `screen`); o **acesso remoto seguro** via **SSH**, incluindo transferência de arquivos com `scp`, configuração de atalhos de host e boas práticas de reforço de segurança; e o domínio do **editor vi**, essencial para editar arquivos de configuração diretamente em servidores remotos, cobrindo seus dois modos de operação, salvamento, navegação, cópia/colagem, exclusão e busca/substituição de texto.

---

# Aula 03 — Recursos Avançados

**Disciplina:** Linux Fundamentos
**Autor do material:** Henrique Poyatos
**Instituição:** FIAP ON

## Objetivos

Aprofundar o conhecimento sobre administração do sistema operacional Linux nos eixos de **gerenciamento de volumes de armazenamento**, **agendamento de tarefas** e **política de firewall**, incluindo a técnica avançada de segurança conhecida como *port-knocking*.

## Conceitos Principais

- Volumes, partições e sistemas de arquivos
- Ferramentas de linha de comando: `fdisk`, `mkfs`, `mount`, `umount`, `fsck`
- Montagem automática via `/etc/fstab`
- Agendamento com `cron`, `anacron` e `crontab`
- Firewall com `iptables`: correntes (*chains*), ações e parâmetros
- Segurança adicional por meio de *port-knocking* com o serviço `knockd`

## Conteúdo da Aula

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

## Exemplos Práticos

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

## Comandos e Código

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

## Boas Práticas e Pontos de Atenção

- Confirmar sempre o disco correto antes de particionar com `fdisk` — a operação é destrutiva.
- Usar **UUID** (ou LABEL) em vez do nome `/dev/sdX` no `/etc/fstab`, já que a numeração pode mudar entre boots.
- Nunca esquecer a regra de aceite para `lo` (loopback) ao configurar firewall remotamente — perder a sessão SSH atual pode significar perder o acesso ao servidor.
- A ordem das regras do `iptables` é determinante: regras específicas de liberação **antes** de regras genéricas de bloqueio.
- Usar `-I` em vez de `-A` quando a nova regra precisa ter prioridade sobre um bloqueio já existente (caso do `knockd.conf`).
- Evitar `chmod 777` em scripts sensíveis; usar permissões restritas (`700`), exclusivas do root.
- Trocar as portas padrão do `knockd` (7000, 8000, 9000) e do SSH (22) para dificultar ataques automatizados.
- Cuidado com regras de firewall baseadas em IP dinâmico: uma mudança de IP pode bloquear o próprio administrador.
- Testar sempre em ambiente controlado (VM) antes de aplicar regras de firewall em produção.

## Resumo da Aula

Esta aula consolidou três pilares da administração avançada de Linux: **gerenciamento de volumes** (particionamento com `fdisk`, formatação com `mkfs`, montagem manual/automática e verificação de integridade com `fsck`); **agendamento de tarefas** com `cron`/`crontab`, incluindo a sintaxe de seis campos e as alternativas simplificadas via `/etc/cron.*`; e **segurança de rede** com `iptables` (correntes, ações, ordem crítica das regras) reforçada pela técnica de **port-knocking** via `knockd`.

---

# Visão Geral do Curso

## Principais Conceitos

O curso **Linux Fundamentos** (FIAP) constrói, ao longo de três aulas, uma trilha progressiva de competências para administração de sistemas Linux:

1. **Aula 01** estabelece a base: comandos essenciais, gerenciamento de usuários, manipulação do sistema de arquivos e configuração básica de rede.
2. **Aula 02** avança para administração operacional: controle de processos, acesso remoto seguro (SSH/SCP) e domínio do editor `vi`.
3. **Aula 03** aprofunda em administração de infraestrutura: gerenciamento de armazenamento, automação de tarefas e segurança de rede (firewall e *port-knocking*).

## Conexão entre os Conteúdos

Os três capítulos formam uma progressão coerente e cumulativa:

- **Comandos básicos → administração → infraestrutura**: os comandos de manipulação de arquivos e diretórios da Aula 01 (`cp`, `mv`, `tar`, `grep`) são reutilizados diretamente na Aula 03 para criar e organizar rotinas de backup automatizadas via `cron`.
- **`nmap`, apresentado na Aula 01** como ferramenta de escaneamento de portas, reaparece na Aula 03 como instrumento de **teste e validação** das regras de firewall e do funcionamento do *port-knocking*.
- **SSH (Aula 02)** é o serviço remoto mais visado por invasores, e as recomendações de segurança apresentadas ali (senhas fortes, restrição de IPs, mudança de porta) são **implementadas tecnicamente** na Aula 03, através de `iptables` e *port-knocking* — o próprio material da Aula 02 antecipa esse encadeamento ao mencionar que o tema será aprofundado "em um capítulo de uma etapa seguinte".
- **O editor `vi` (Aula 02)** é a ferramenta usada, na prática, para criar e editar os scripts de firewall (`/etc/firewall.sh`) e os arquivos de configuração (`/etc/knockd.conf`, `/etc/fstab`, `/etc/crontab`) apresentados na Aula 03 — sem o domínio do vi, a administração remota desses arquivos seria inviável.
- **`sudo`, gerenciamento de usuários e permissões (Aula 01)** permeiam toda a Aula 03, já que operações de particionamento, montagem de volumes e configuração de firewall exigem privilégios de superusuário.
- O documento **`/etc/init.d/<serviço> start`**, usado para subir o Apache (Aula 02) e o SSH (Aula 02), segue o mesmo padrão utilizado para subir o `knockd` na Aula 03, reforçando a familiaridade do aluno com o gerenciamento de serviços via *daemon*.

## Conhecimentos Adquiridos

Ao concluir o curso, o estudante é capaz de:

- Operar o terminal Linux com autonomia, utilizando os comandos essenciais de navegação, manipulação de arquivos, gerenciamento de usuários e configuração de rede.
- Monitorar e controlar processos em execução, incluindo a execução de tarefas de longa duração de forma resiliente com `screen`.
- Estabelecer e proteger conexões remotas via SSH, e transferir arquivos com segurança via SCP.
- Editar arquivos de configuração diretamente no terminal utilizando o editor `vi`.
- Gerenciar volumes de armazenamento — particionar, formatar, montar (manual e automaticamente) e verificar a integridade de sistemas de arquivos.
- Automatizar tarefas recorrentes com `cron`/`crontab`.
- Implementar e testar políticas de firewall com `iptables`, incluindo uma camada adicional de segurança com *port-knocking*.

## Conclusão

O curso Linux Fundamentos oferece uma base técnica sólida e progressiva para a administração de sistemas Linux em ambientes reais — desde a operação básica do terminal até a implementação de políticas avançadas de segurança de rede. A ênfase em exemplos práticos, executados em ambientes virtualizados (Oracle VirtualBox) e em provedores de nuvem (Digital Ocean) e hardware embarcado (Raspberry Pi), aproxima o conteúdo teórico da realidade profissional de administradores de sistemas, engenheiros de infraestrutura e profissionais de DevOps, formando um alicerce indispensável para tópicos futuros mais avançados de administração e segurança de sistemas Linux.
