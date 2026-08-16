# 🐧 Linux Fundamentos

[← Voltar ao índice do repositório](../README.md)

**Plataforma:** FIAP ON — Eu Capacito · **Categoria:** Development · **Carga horária:** 40h · **Capítulos:** 3

<p>
  <img src="https://img.shields.io/badge/Nota-100%2F100-2ea44f?style=flat-square" alt="nota"/>
  <img src="https://img.shields.io/badge/Status-Conclu%C3%ADdo-2ea44f?style=flat-square" alt="status"/>
</p>

## Descrição

Curso introdutório sobre o sistema operacional Linux, cobrindo terminal e comandos essenciais, gerenciamento de usuários e pacotes, manipulação de arquivos, administração de sistema, acesso remoto via SSH, edição de texto em linha de comando (`vi`), gerenciamento de volumes, agendamento de tarefas (`cron`) e política de firewall (`iptables`).

## Objetivos

Ao final do curso, o estudante é capaz de:

- Operar o terminal Linux com autonomia, utilizando os comandos essenciais de navegação, manipulação de arquivos, gerenciamento de usuários e configuração de rede.
- Monitorar e controlar processos em execução, incluindo a execução de tarefas de longa duração de forma resiliente com `screen`.
- Estabelecer e proteger conexões remotas via SSH, e transferir arquivos com segurança via SCP.
- Editar arquivos de configuração diretamente no terminal utilizando o editor `vi`.
- Gerenciar volumes de armazenamento — particionar, formatar, montar (manual e automaticamente) e verificar a integridade de sistemas de arquivos.
- Automatizar tarefas recorrentes com `cron`/`crontab`.
- Implementar e testar políticas de firewall com `iptables`, incluindo uma camada adicional de segurança com *port-knocking*.

## Pré-requisitos

Nenhum conhecimento prévio de Linux é exigido. Familiaridade básica com conceitos de sistemas operacionais e navegação em terminal (de qualquer SO) ajuda, mas não é obrigatória.

## Público-alvo

Profissionais e estudantes de TI que precisam operar servidores Linux no dia a dia — suporte técnico, infraestrutura, DevOps — e qualquer pessoa que queira migrar de uma administração baseada em interface gráfica para uma baseada em terminal.

## Estrutura do curso

O curso é dividido em 3 aulas, com progressão cumulativa: comandos básicos → administração operacional → infraestrutura avançada.

| # | Aula | Resumo |
|---|---|---|
| 01 | [Introdução e Primeiros Comandos](aula-01/conteudo.md) | Comandos essenciais, gerenciamento de usuários e pacotes, manipulação de arquivos/diretórios e configuração básica de rede. |
| 02 | [Administração de Sistema e Editor de Texto](aula-02/conteudo.md) | Controle de processos, acesso remoto seguro via SSH/SCP e domínio do editor `vi`. |
| 03 | [Recursos Avançados](aula-03/conteudo.md) | Gerenciamento de volumes, automação de tarefas com `cron` e segurança de rede com `iptables`/*port-knocking*. |

Cada aula contém:
- `conteudo.md` — material de estudo consolidado da aula.
- `exercicios.md` — 10 exercícios de fixação, com dificuldade progressiva.
- o PDF oficial do capítulo correspondente.

## Visão geral dos principais conceitos

1. **Aula 01** estabelece a base: comandos essenciais, gerenciamento de usuários, manipulação do sistema de arquivos e configuração básica de rede.
2. **Aula 02** avança para administração operacional: controle de processos, acesso remoto seguro (SSH/SCP) e domínio do editor `vi`.
3. **Aula 03** aprofunda em administração de infraestrutura: gerenciamento de armazenamento, automação de tarefas e segurança de rede (firewall e *port-knocking*).

As três aulas formam uma progressão coerente e cumulativa — por exemplo, o `nmap` (Aula 01) reaparece na Aula 03 para validar regras de firewall, e as recomendações de segurança de SSH (Aula 02) são implementadas tecnicamente via `iptables`/*port-knocking* na Aula 03. Os detalhes completos dessas conexões estão descritos na seção "Relação com Outras Aulas" de cada `conteudo.md`.

## Orientações de estudo

- Siga a ordem das aulas — o conteúdo é cumulativo e cada aula pressupõe o domínio da anterior.
- Resolva os `exercicios.md` de cada aula antes de avançar para a próxima.
- Use o [`questionario.md`](questionario.md) como autoavaliação final, depois de concluir as três aulas.
- Os exemplos práticos do material oficial foram executados em VirtualBox, Digital Ocean e Raspberry Pi — reproduzir os comandos em uma VM própria (ex.: Debian em VirtualBox) reforça o aprendizado.

## Conclusão

O curso Linux Fundamentos oferece uma base técnica sólida e progressiva para a administração de sistemas Linux em ambientes reais — desde a operação básica do terminal até a implementação de políticas avançadas de segurança de rede, formando um alicerce indispensável para tópicos futuros de administração e segurança de sistemas.

---

## Arquivos deste curso

- [`questionario.md`](questionario.md) — avaliação final autoral do curso (10 questões novas, integrando as três aulas).
- [`avaliacao-oficial.md`](avaliacao-oficial.md) — registro histórico da prova de certificação oficial da FIAP ON (nota, gabarito comentado das 20 questões e certificado).
- `certificado.png` — imagem do certificado emitido pela FIAP.
