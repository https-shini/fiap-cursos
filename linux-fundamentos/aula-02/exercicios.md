# Exercícios — Aula 02: Administração de Sistema e Editor de Texto

[← Voltar ao índice do curso](../README.md) · [Conteúdo da aula](conteudo.md)

10 exercícios, com dificuldade progressiva, sobre o conteúdo desta aula.

---

### 1. (Fácil) Monitoramento de processos

Qual comando exibe, em tempo real, o consumo de CPU, memória RAM e swap de todos os processos em execução?

---

### 2. (Fácil) Entrando em edição no vi

Você abriu um arquivo com `vi config.txt` e precisa começar a digitar texto no início da linha atual. Qual tecla usar para entrar no modo de edição nesse ponto específico da linha?

---

### 3. (Fácil) Encerrando um processo

Um processo com PID `4521` parou de responder. Qual comando você usa para encerrá-lo?

---

### 4. (Intermediário) Processo em segundo plano

Você precisa iniciar um script `monitor.sh` que demora horas para terminar, sem travar o terminal para outros comandos. Qual símbolo/comando você usa, e como confirma depois que o processo continua rodando?

---

### 5. (Intermediário) Tarefa persistente

Você está conectado via SSH e precisa rodar uma migração de banco de dados que leva 3 horas, mas sabe que a conexão pode cair antes disso. Qual ferramenta apresentada na aula resolve esse problema, e como você a usaria (comandos de criação e de retomada da sessão)?

---

### 6. (Intermediário) Transferência segura de arquivo

Você precisa enviar o arquivo `relatorio.pdf`, que está na sua máquina local, para o diretório `/home/usuario/docs/` de um servidor remoto com IP `203.0.113.10`. Escreva o comando completo.

---

### 7. (Intermediário/Avançado) Edição no vi

Usando o `vi`, você abriu um arquivo de configuração de 200 linhas e precisa: (a) ir diretamente para a linha 87, (b) substituir todas as ocorrências da palavra `producao` por `homologacao` no arquivo inteiro, com confirmação a cada troca, e (c) salvar e sair. Escreva a sequência de comandos do `vi` para cada etapa.

---

### 8. (Intermediário/Avançado) Segurança de acesso remoto

Um servidor está com a porta 22 (SSH) exposta publicamente com autenticação por senha. Cite duas medidas de segurança apresentadas na aula que reduziriam o risco de ataques automatizados a esse serviço, explicando o efeito prático de cada uma.

---

### 9. (Difícil) Diagnóstico de processo

Você percebe que o servidor está com uso de CPU anormalmente alto. Descreva o fluxo de investigação: qual comando você usaria primeiro para identificar o(s) processo(s) responsável(is), quais informações da saída seriam relevantes, e qual comando usaria para encerrar o processo de forma controlada antes de recorrer a uma finalização forçada.

---

### 10. (Difícil) Integração SSH + vi + processos

Você precisa, via SSH, editar remotamente um arquivo de configuração de um serviço, reiniciar esse serviço em segundo plano e, em seguida, encerrar a sessão SSH sem interromper o processo que acabou de iniciar. Descreva o fluxo completo de comandos, indicando por que uma ferramenta apresentada nesta aula é necessária para que o processo sobreviva ao fim da sessão.
