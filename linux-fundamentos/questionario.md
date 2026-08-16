# Questionário Final — Linux Fundamentos

[← Voltar ao índice do curso](README.md)

Avaliação final autoral do curso, com 10 questões novas e independentes dos exercícios de cada aula. O foco está em integração entre aulas, análise de cenários e resolução de problemas — não em memorização de definições.

> Este questionário é diferente da [avaliação oficial da FIAP ON](avaliacao-oficial.md), cujo gabarito de 20 questões é um registro histórico da prova de certificação já realizada.

---

### 1. Cenário de provisionamento completo

Você recebeu um servidor Debian recém-instalado e precisa deixá-lo pronto para produção: criar um usuário de operação sem privilégios de root, instalar o pacote `nginx`, e confirmar que o serviço responde na porta 80 a partir de outra máquina. Descreva a sequência de comandos, indicando de qual aula vem cada grupo de comandos.

### 2. Diagnóstico sob pressão

Um servidor está lento. Você suspeita de um processo consumindo CPU excessiva, mas também quer confirmar se o disco de dados está com espaço suficiente. Quais comandos você usaria para cada verificação, e em que ordem investigaria?

### 3. Segurança em camadas

Explique como as três aulas do curso se combinam para proteger um servidor exposto à internet: o que a Aula 01 contribui, o que a Aula 02 contribui e o que a Aula 03 acrescenta. Use um exemplo concreto de ataque que cada camada mitiga.

### 4. Análise de erro em produção

Um colega tentou liberar acesso a uma porta específica no firewall, mas a regra ficou "sem efeito" mesmo após reiniciar o `iptables`. Ele mostra a regra: `iptables -A INPUT -s 10.0.0.5 -p tcp --dport 22 -j ACCEPT`, sabendo que já existe uma regra anterior bloqueando todo o tráfego na porta 22. Diagnostique o problema e proponha a correção, explicando o motivo técnico.

### 5. Automação com verificação

Projete uma rotina que faça backup diário do diretório `/etc`, verifique automaticamente se o backup foi criado (por exemplo, checando se o arquivo do dia existe) e, em caso de falha, registre isso em um log. Quais mecanismos das três aulas você combinaria, mesmo que parte da lógica de verificação exija scripts além do que foi ensinado no curso?

### 6. Trade-off de acesso remoto

Compare, com argumentos técnicos, os cenários em que faz sentido usar SSH puro versus SSH com *port-knocking* adicional. Em que tipo de ambiente (startup pequena, empresa com equipe de segurança dedicada, servidor pessoal doméstico) cada abordagem é mais adequada, e por quê?

### 7. Recuperação de desastre

Um disco de dados `/dev/sdb1`, montado em `/media/dados`, apresenta erros de leitura ao ser acessado após um desligamento incorreto do servidor. Descreva o procedimento de diagnóstico e recuperação, incluindo os riscos de cada etapa.

### 8. Editor como ferramenta de infraestrutura

Explique por que o domínio do `vi` é considerado, no curso, pré-requisito prático para a administração de firewall e agendamento de tarefas (Aula 03) — e não apenas uma habilidade isolada da Aula 02.

### 9. Projeto de política de firewall

Você precisa montar a política de firewall completa de um servidor que roda um site (porta 80/443) e é administrado remotamente por SSH (porta customizada 2222), bloqueando todo o restante do tráfego de entrada. Escreva as regras de `iptables`, na ordem correta, e justifique a ordem escolhida.

### 10. Síntese — do zero à infraestrutura segura

Resuma, em um fluxo único, o caminho que este curso percorreu: desde a primeira vez que um usuário abre um terminal Linux até a implementação de uma política de firewall com *port-knocking*. Identifique os três ou quatro pontos de virada mais importantes dessa jornada e explique por que cada um foi necessário para o seguinte.
