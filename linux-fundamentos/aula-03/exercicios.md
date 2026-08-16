# Exercícios — Aula 03: Recursos Avançados

[← Voltar ao índice do curso](../README.md) · [Conteúdo da aula](conteudo.md)

10 exercícios, com dificuldade progressiva, sobre o conteúdo desta aula.

---

### 1. (Fácil) Identificação de discos

Qual diretório do sistema Linux é usado para localizar e identificar os discos (HDD/SSD) conectados à máquina?

---

### 2. (Fácil) Verificação de integridade

Antes de montar uma partição que pode estar corrompida, qual comando você deve executar para verificar e corrigir o sistema de arquivos?

---

### 3. (Fácil) Correntes do iptables

Cite as três correntes (*chains*) padrão do `iptables` e o que cada uma controla.

---

### 4. (Intermediário) Provisionamento de um novo disco

Um novo disco `/dev/sdb` foi conectado ao servidor e precisa ser particionado (uma única partição de 10 GB), formatado em ext4 e montado em `/media/dados`. Escreva a sequência completa de comandos, na ordem correta.

---

### 5. (Intermediário) Montagem automática

Depois de montar manualmente a partição do exercício anterior, você quer que ela seja montada automaticamente a cada reinicialização do servidor. O que precisa ser feito, e por que é recomendado usar UUID em vez do caminho `/dev/sdb1` diretamente?

---

### 6. (Intermediário) Agendamento de backup

Escreva uma entrada de `crontab` que execute, todos os dias às 2h da manhã, um backup do diretório `/etc` compactado em `tar.gz` com a data no nome do arquivo.

---

### 7. (Intermediário/Avançado) Ordem de regras no firewall

Um administrador configurou o `iptables` para bloquear todo o tráfego na porta 80 com `iptables -A INPUT -p tcp --dport 80 -j REJECT`, mas depois quis liberar o acesso apenas para o IP `10.0.0.5` adicionando uma nova regra de aceite com `-A`. A liberação não funcionou. Explique por que, e qual seria a correção.

---

### 8. (Intermediário/Avançado) Risco de configuração remota

Você está configurando regras de `iptables` remotamente, via SSH, para restringir o acesso ao servidor. Qual erro comum apresentado na aula pode fazer você perder o próprio acesso ao servidor durante essa configuração, e como evitá-lo?

---

### 9. (Difícil) Projetando uma política de firewall com port-knocking

Você precisa proteger a porta SSH de um servidor exposto na internet, mantendo-a fechada por padrão e liberando o acesso apenas para quem conhece uma sequência secreta de portas. Descreva a arquitetura da solução (regra padrão do `iptables`, papel do `knockd`, sequência de configuração em `knockd.conf` e a regra de exceção necessária) e explique por que essa regra de exceção precisa usar `-I` em vez de `-A`.

---

### 10. (Difícil) Cenário integrado — infraestrutura completa

Um novo servidor de backup precisa: (a) ter um segundo disco particionado e montado permanentemente para armazenar os arquivos; (b) executar rotinas automáticas de backup do diretório `/etc` todas as noites; e (c) ter a porta SSH protegida com *port-knocking*, considerando que a administração será feita remotamente. Descreva, de forma integrada, os passos e comandos das três frentes, indicando a ordem lógica de execução e os riscos que você tomaria cuidado para não perder o próprio acesso ao servidor durante o processo.
