# Exercícios — Aula 01: Introdução e Primeiros Comandos

[← Voltar ao índice do curso](../README.md) · [Conteúdo da aula](conteudo.md)

10 exercícios, com dificuldade progressiva, sobre o conteúdo desta aula.

---

### 1. (Fácil) Ajuda embutida

Você recebeu acesso a um servidor Debian e precisa entender rapidamente o que o comando `chmod` faz e quais parâmetros aceita, sem sair do terminal. Qual comando você digita?

---

### 2. (Fácil) Usuário vs. superusuário

Observando o prompt `guilherme@srv01:/var/log#`, responda: esse usuário está operando como usuário comum ou como root? Justifique com base no símbolo do prompt.

---

### 3. (Fácil) Gerenciamento de pacotes

Você precisa instalar o pacote `htop` em um servidor Debian que acabou de ser provisionado. Escreva a sequência de comandos necessária, começando pela atualização da lista de repositórios.

---

### 4. (Intermediário) Diagnóstico de comando

Um colega executou `LS -L` em um servidor Debian e recebeu erro de "comando não encontrado", mesmo o comando `ls -l` funcionando normalmente. Explique o motivo do erro.

---

### 5. (Intermediário) Manipulação de arquivos

Escreva a sequência de comandos para: criar um diretório chamado `backup-logs`, copiar todos os arquivos `.log` do diretório atual para dentro dele, e depois compactar esse diretório inteiro em um arquivo `backup-logs.tar.gz`.

---

### 6. (Intermediário) Busca de conteúdo

Você precisa encontrar, dentro do arquivo `/var/log/auth.log`, todas as linhas que contenham a palavra `Failed` (falhas de autenticação). Qual comando resolve isso?

---

### 7. (Intermediário/Avançado) Usuários e grupos

Um novo colaborador chamado `rebeka` precisa ser cadastrado no sistema e adicionado a um grupo já existente chamado `suporte`, sem ter privilégios de administrador. Descreva o(s) comando(s) necessário(s), explicando a função de cada um.

---

### 8. (Intermediário/Avançado) Rede básica

Você precisa configurar manualmente a interface de rede `enp0s5` com o IP `192.168.1.50` e máscara `255.255.255.0`, e depois confirmar se a porta 22 (SSH) está acessível em `192.168.1.100`. Quais dois comandos, na ordem certa, resolvem essa tarefa?

---

### 9. (Difícil) Análise de cenário — privilégios

Um script `deploy.sh` precisa ser executado apenas por usuários do grupo `devops`, sem exigir troca de usuário nem digitação de senha adicional a cada execução, mas sem tornar o script executável por qualquer pessoa do sistema. Considerando os comandos e conceitos desta aula (usuários, grupos e permissões via `usermod`), explique qual seria a abordagem — e por que simplesmente usar `chmod 777 deploy.sh` seria uma má prática.

---

### 10. (Difícil) Comparação de abordagens

Compare, com argumentos técnicos baseados no conteúdo da aula, por que a administração de um servidor Linux remoto via SSH costuma ser preferível a uma ferramenta de acesso remoto gráfico (como o TeamViewer) em cenários de conexão de internet limitada — e em que situação você recomendaria, ainda assim, uma interface gráfica.
