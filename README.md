<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=24&height=120&section=header&text=FIAP%20ON%20%7C%20Trilha%20de%20Nano%20Courses%20Gratuitos&fontSize=28&fontColor=ffffff" alt="banner"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Em%20andamento-yellow?style=for-the-badge" alt="status"/>
  <img src="https://img.shields.io/badge/Cursos%20concluídos-1%20%2F%2028-2ea44f?style=for-the-badge" alt="cursos concluidos"/>
  <img src="https://img.shields.io/badge/Carga%20horária%20total-1.820h-3178c6?style=for-the-badge" alt="carga horaria"/>
</p>

<p align="center">
  <a href="#-sobre-este-repositório">Sobre</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#-objetivos-de-aprendizagem">Objetivos</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#-cursos">Cursos</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="trilha-de-estudos.md">Catálogo Completo</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#-portfólio--do-curso-ao-projeto">Portfólio</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#️-arquitetura-do-repositório">Arquitetura</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="_templates">Templates</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#-licença">Licença</a>
</p>

## 📖 Sobre este repositório

Repositório dedicado ao registro dos **Nano Courses gratuitos da FIAP ON** (plataforma Eu Capacito), reunindo material oficial, conteúdo de estudo em Markdown, exercícios autorais, questionários finais e certificados de cada curso.

O repositório documenta uma trilha de estudos cobrindo programação, infraestrutura, DevOps, cloud, segurança e dados — sempre com o objetivo de transformar cada certificado em algo publicável e demonstrável, não apenas em uma credencial isolada.

**Plataforma:** FIAP ON — Eu Capacito ([on.fiap.com.br/nano-courses](https://on.fiap.com.br/nano-courses/)) <br>
**Perfil de origem:** Suporte Técnico, com experiência em Python, JavaScript/React, Node.js, banco de dados, Git e infraestrutura.

---

## 🎯 Objetivos de aprendizagem

- **Cognitivos:** consolidar fundamentos de programação, infraestrutura, DevOps, cloud, segurança e dados a partir de uma base prática de Suporte Técnico.
- **Habilidades:** aplicar cada curso em artefatos reais — scripts, pipelines, documentação de projetos, deploys — em vez de acumular certificados soltos.
- **Atitudes:** construir uma narrativa de carreira coerente que sustente o portfólio e as entrevistas técnicas.

---

## 📊 Cursos

| Curso | Status | Aulas | Horas |
|---|---|---:|---:|
| [Linux Fundamentos](./linux-fundamentos/README.md) | Concluído | 3 | 40h |
| [Python Development](./python-development/README.md) | Em Andamento | 6 | 80h |
| [Java Development](./java-development/README.md) | Em Andamento | 6 | 60h |
| [Cybersecurity](./cybersecurity/README.md) | Concluído | 11 | 120h |

"Não iniciado" reflete que o material oficial (PDFs) já está versionado na estrutura padrão, mas o conteúdo em Markdown de cada aula ainda não foi processado — ver o status detalhado no README de cada curso.

### 🔜 Próximos (em ordem de prioridade, após os quatro acima)

- [ ] DevOps & Agile Culture (60h)
- [ ] Python (80h)
- [ ] Learn to Program (60h)
- [ ] Engenharia de Software (100h)
- [ ] Gestão de Infraestrutura de TI (20h)
- [ ] Cloud Fundamentals, Administration and Solution Architect (80h)

### 🗺️ Catálogo completo

A lista integral dos **28 cursos** disponíveis na plataforma — organizada por categoria oficial (Business, Cloud, Data Science, Design, Development, Innovation, Marketing, Security), com horas, capítulos e resumo de cada um — está em:

👉 [**`trilha-de-estudos.md`** — Catálogo completo de cursos FIAP ON](trilha-de-estudos.md)

---

## 💼 Portfólio — do curso ao projeto

Certificado sozinho não vira portfólio. Esta tabela cobre apenas os **9 cursos das Fases 1–3** — o núcleo prioritário para a evolução de carreira (~640h). O catálogo completo, com ideias de artefato para os demais 19 cursos, está em [`trilha-de-estudos.md`](trilha-de-estudos.md).

Esses 9 cursos foram escolhidos por representarem, ao mesmo tempo, a maior prioridade estratégica e o maior interesse pessoal. Por isso, receberão atenção especial: serão documentados em detalhe e estudados antes dos demais.

#### Fundamentos de programação

| Curso | Artefato de portfólio |
|---|---|
| [Python Development](python-development/README.md) | Script/CLI ou mini-API que resolva um problema real próprio. |
| Python | Segundo projeto prático explorando recursos diferentes do Development (ex.: manipulação de dados, testes). |
| Learn to Program | Coleção de exercícios de lógica resolvidos, publicada como repositório de estudo. |

#### Engenharia e infraestrutura

| Curso | Artefato de portfólio |
|---|---|
| Engenharia de Software | Documentar um projeto existente (ex.: Portal-Receitas) com requisitos e diagramas. |
| [**Linux Fundamentos**](linux-fundamentos/README.md) ✅ | Este repositório: conteúdo consolidado por aula + exercícios + questionário + certificado. |
| Gestão de Infraestrutura de TI | Runbook/checklist de infraestrutura aplicado a um ambiente pessoal (home lab, VPS), publicado. |

#### DevOps, Cloud e Segurança

| Curso | Artefato de portfólio |
|---|---|
| DevOps & Agile Culture | Pipeline CI/CD simples (GitHub Actions) documentado em repositório. |
| Cloud Fundamentals, Administration and Solution Architect | Deploy de um projeto próprio numa cloud gratuita, com post curto explicando o processo. |
| [Cybersecurity](cybersecurity/README.md) ✅ | Resumo/anotação pública (LinkedIn ou blog) sobre boas práticas aplicadas. |

> `Java Development` não integra as Fases 1–3 priorizadas, mas já tem estrutura criada por já possuir material oficial baixado.

---

## 🏗️ Arquitetura do repositório

Princípio estrutural: **Curso → Aula → Conteúdo + Exercícios**, com informações gerais e avaliação final centralizadas no nível do curso.

```
<curso-em-kebab-case>/
├── README.md               # índice e visão geral do curso
├── questionario.md          # avaliação final autoral (10 questões novas, integrando as aulas)
├── aula-01/
│   ├── conteudo.md          # material de estudo consolidado da aula
│   ├── exercicios.md        # exatamente 10 exercícios, dificuldade progressiva
│   └── <slug>-01-<tema>.pdf # material oficial da aula
├── aula-02/
│   └── ...
└── ...
```

- **Diretórios de curso** usam `kebab-case` sem espaços (ex.: `linux-fundamentos`), evitando os problemas de link com `%20` que a estrutura anterior apresentava.
- **Diretórios de aula** seguem `aula-01`, `aula-02`, ... com zero à esquerda, garantindo ordenação lexicográfica correta mesmo além de 10 aulas.
- **`README.md`** de cada curso funciona como índice: descrição, objetivos, pré-requisitos, lista de aulas com links, orientações de estudo.
- **`conteudo.md`** de cada aula reúne teoria, exemplos, cheat sheet/aplicações práticas, boas práticas e um resumo — seções usadas conforme a relevância para o conteúdo, sem preencher artificialmente.
- **`exercicios.md`** de cada aula contém exatamente 10 exercícios com dificuldade progressiva (fácil → difícil), exclusivos ao conteúdo daquela aula.
- **`questionario.md`**, no nível do curso, é uma avaliação final nova e independente dos exercícios de cada aula, com foco em integração entre aulas e resolução de problemas.

### Exceção justificada: `avaliacao-oficial.md`

Cursos que possuem uma **prova de certificação oficial já realizada** na plataforma (com nota, tentativas e certificado real) preservam esse registro em `avaliacao-oficial.md`, no nível do curso, além do `questionario.md`. São documentos de natureza diferente — um é um registro histórico de credencial externa; o outro é uma avaliação de estudo autoral — e por isso não foram fundidos. Hoje só `Linux-Fundamentos/` tem esse arquivo; qualquer curso futuro que inclua uma prova oficial similar deve seguir o mesmo padrão.

### Templates

Os quatro arquivos reutilizáveis (`README.md`, `conteudo.md`, `exercicios.md`, `questionario.md`) têm modelos genéricos em [`_templates/`](_templates), usados como ponto de partida ao criar um curso ou aula nova, sem exigir decisões estruturais adicionais.

---

## 📄 Licença

Distribuído sob a **Licença MIT**. Veja o arquivo [`LICENSE`](LICENSE) para mais informações.

> *Este repositório reúne, de forma organizada, as atividades, estudos, materiais e práticas desenvolvidos ao longo da trilha de estudos dos Nano Courses gratuitos da FIAP ON. Seu objetivo é servir como fonte de consulta e apoio ao aprendizado contínuo, contribuindo para o desenvolvimento profissional e a evolução da carreira.*

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=24&height=100&section=footer" alt="footer"/>
