# Engenharia de Dados FAIR — versão compacta (16h)

Material do curso **Engenharia de Dados FAIR**, ministrado por **Luiz Olavo Bonino** no **IBICT**, em Brasília, de **22 a 25 de junho de 2026**.

O curso oferece uma visão panorâmica dos princípios FAIR e do processo de FAIRificação de dados, combinando exposição teórica com sessões práticas. É voltado a um público de Ciência da Informação com formações variadas.

## Objetivos

- Estabelecer um entendimento comum dos princípios FAIR.
- Familiarizar-se com o processo de FAIRificação.
- Familiarizar-se com as técnicas e tecnologias envolvidas.
- Adquirir experiência na avaliação da FAIRness de objetos digitais.
- Adquirir experiência prática na FAIRificação de dados.

## Distribuição do tempo

| Tipo de atividade | Horas | Proporção |
|---|---|---|
| Exposição (aulas teóricas) | 7h 10min | ~45% |
| Sessões práticas | 6h 00min | ~37% |
| Discussão, abertura e encerramento | 2h 50min | ~18% |
| **Total** | **16h** | **100%** |

## Programa e calendário

O curso tem 4 sessões de 4 horas, uma por dia:

| Sessão | Data | Tema | Slides (PDF) |
|---|---|---|---|
| Sessão 1 | 22 de junho (seg) | Introdução à iniciativa FAIR e princípios FAIR | `slides/Sessão 1.1 …`, `1.2 …`, `1.3 …` |
| Sessão 2 | 23 de junho (ter) | Processo de FAIRificação e modelagem semântica de dados | `slides/Sessão 2 …` |
| Sessão 3 | 24 de junho (qua) | Metadados e modelagem de metadados | `slides/Sessão 3.1 …`, `3.2 …` |
| Sessão 4 | 25 de junho (qui) | Publicação e avaliação de recursos FAIR | `slides/Sessão 4.1 …`, `4.2 …` |

### Sessão 1 — Introdução à iniciativa FAIR e princípios FAIR (22 jun)

Abertura e objetivos; a iniciativa FAIR, motivação e desenvolvimentos atuais (GO FAIR, EOSC, mandatos de financiadores); os 15 princípios FAIR explicados em profundidade (F, A, I, R); prática de seleção do conjunto de dados do curso e avaliação intuitiva inicial de FAIRness; discussão dos desafios identificados.

### Sessão 2 — Processo de FAIRificação e modelagem semântica de dados (23 jun)

Processo de FAIRificação (etapas, papéis, ferramentas) e planejamento; modelagem semântica de dados (RDF, RDFS, OWL essencial, vocabulários e ontologias reutilizáveis — FOAF, SKOS, schema.org); ferramentas (Protégé, OpenRefine); sessão prática de modelagem semântica do conjunto escolhido e discussão dos modelos.

### Sessão 3 — Metadados e modelagem de metadados (24 jun)

O que são metadados, tipos (descritivos, estruturais, administrativos) e padrões (Dublin Core, DataCite, schema.org, padrões de domínio); modelagem semântica de metadados — esquemas e perfis de aplicação, reutilização de vocabulários, relação entre modelo de dados e de metadados, identificadores; introdução a SHACL; sessão prática de modelagem de metadados e revisão por pares.

### Sessão 4 — Publicação e avaliação de recursos FAIR (25 jun)

Identificadores persistentes (DOI, Handle, ORCID), repositórios (Zenodo, Dataverse), licenças e protocolos de acesso; prática de publicação (em sandbox) do conjunto FAIRificado; métricas e ferramentas de avaliação de FAIRness (F-UJI, FAIR Evaluator, modelos de maturidade); prática de avaliação do próprio dataset e do de um par; apresentações e encerramento.

## Estrutura do repositório

```
curso-fair-ibict-2026/
├── README.md
├── slides/   — os 8 conjuntos de slides do curso, em PDF
└── dados/    — conjunto de dados condutor "World Dishes" usado nas práticas
```

## Conjunto de dados condutor — "World Dishes"

Conjunto sintético criado para o curso, usado como exemplo condutor da Sessão 1 à Sessão 4. Ele é **deliberadamente não-FAIR** (identificadores ausentes, valores inconsistentes, sem metadados ricos, sem licença) para que os participantes o FAIRifiquem ao longo dos quatro dias.

- `dados/dishes.csv` — 55 pratos, com campos inconsistentes propositalmente (nomes de país variados, unidades mistas, valores ausentes).
- `dados/countries.csv` — tabela de referência de países (ISO3, região canônica, QID do Wikidata) para reconciliação.
- `dados/GUIA_Exemplo_Condutor.md` — guia do instrutor: problemas plantados em cada campo e como eles conectam com cada princípio FAIR.

## Instrutor

Luiz Olavo Bonino — l.o.boninodasilvasantos@utwente.nl
