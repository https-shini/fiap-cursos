# Aula 02 — Administração de Sistema e Editor de Texto

[← Voltar ao índice do curso](../README.md)

**Disciplina:** Linux Fundamentos
**Autor do material:** Filipi Pires
**Instituição:** FIAP ON

## Objetivos de Aprendizagem

Aprofundar o conhecimento em administração de sistemas Linux por meio do gerenciamento de processos, do acesso remoto seguro via SSH e do domínio do editor de texto em linha de comando **vi**, ferramenta essencial para edição de arquivos de configuração em servidores sem interface gráfica.

## Conceitos Fundamentais

- Processos: o que são, como monitorá-los e como encerrá-los
- Execução de comandos em primeiro e segundo plano (*foreground*/*background*)
- Terminais virtuais persistentes com `screen`
- Acesso remoto seguro com SSH e transferência de arquivos com SCP
- Reforço de segurança do serviço SSH
- Fundamentos do editor de texto **vi**: modos de operação, edição, navegação, cópia/colagem, busca e substituição

## Conteúdo

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

## Exemplos

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

## Aplicações Práticas

### Cheat sheet de comandos

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

## Boas Práticas

- Preferir `sudo kill`/`killall` apenas quando o processo pertencer a outro usuário — se for o processo do próprio usuário, `sudo` é desnecessário.
- Usar `kill -SIGSTOP`/`-SIGCONT` para liberar recursos temporariamente em vez de encerrar (e perder) processos longos.
- Lembrar que processos em `&` (background) continuam vinculados ao terminal: fechar o terminal os encerra. Para independência real do terminal, usar `screen`.
- Sempre verificar a *fingerprint* apresentada no primeiro acesso SSH a um host.
- Usar `>>` (nunca `>`) ao editar arquivos de configuração como `/etc/hosts` via `echo`, para não apagar o conteúdo existente.
- Reforçar a segurança do SSH: senhas fortes, desabilitar login direto do root, restringir usuários/IPs, trocar a porta padrão.
- No vi, sempre confirmar em qual modo se está (comando ou edição) antes de navegar com as setas, para evitar poluir o documento com caracteres indesejados.
- Lembrar que `u` desfaz **apenas o último** comando no vi — não há histórico de múltiplos "undos" nesta versão básica apresentada.

## Resumo

A aula cobriu três frentes complementares da administração prática de sistemas Linux: o **gerenciamento de processos** (visualização com `ps`/`top`, encerramento com `kill`/`killall`, execução em segundo plano com `&` e terminais persistentes com `screen`); o **acesso remoto seguro** via **SSH**, incluindo transferência de arquivos com `scp`, configuração de atalhos de host e boas práticas de reforço de segurança; e o domínio do **editor vi**, essencial para editar arquivos de configuração diretamente em servidores remotos, cobrindo seus dois modos de operação, salvamento, navegação, cópia/colagem, exclusão e busca/substituição de texto.

---

## Principais Pontos a Memorizar

- `ps` lista processos (comumente com `-a`); `top` monitora processos em tempo real (CPU, RAM, swap); `kill`/`killall` encerram processos por PID ou nome.
- `&` executa um processo em segundo plano; `screen` cria terminais virtuais persistentes que sobrevivem à desconexão (`[CTRL]+[A]` depois `[D]` para *detach*).
- SSH substituiu o Telnet por transmitir dados criptografados; `scp` transfere arquivos com segurança sobre o mesmo protocolo.
- Boas práticas de segurança em SSH: senhas fortes, restrição de IPs e troca da porta padrão — implementadas tecnicamente na Aula 03 com `iptables` e *port-knocking*.
- O `vi` tem dois modos: **comando** e **edição de texto** — entrar em edição com `i`/`a`/`A`/`o`, sair com `[ESC]`.
- Salvar e sair do `vi`: `:w` (salvar), `:wq` ou `:x` (salvar e sair), `:q!` (sair sem salvar).
- Navegação e edição no `vi`: `:set nu` (numerar linhas), `:N` (ir para linha N), `yy`/`p` (copiar/colar linha), `dd` (apagar linha), `u` (desfazer), `/termo` (buscar), `:%s/antigo/novo/gc` (substituir).

## Relação com Outras Aulas

O controle de processos e o acesso remoto via SSH construídos aqui preparam o terreno para a **Aula 03**: as recomendações de segurança para SSH (senhas fortes, restrição de IPs, mudança de porta) são implementadas tecnicamente mais adiante através de `iptables` e *port-knocking*. O editor `vi`, apresentado nesta aula, é a ferramenta usada na prática para editar os scripts de firewall e os arquivos de configuração (`/etc/knockd.conf`, `/etc/fstab`, `/etc/crontab`) da **Aula 03**. Os comandos básicos de manipulação de arquivos e o uso de `sudo`, vistos na **Aula 01**, são pré-requisitos diretos para tudo o que é feito aqui.
