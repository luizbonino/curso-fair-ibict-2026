# Exemplo condutor do curso — *World Dishes* (guia do instrutor)

Conjunto de dados **original e sintético** criado para servir de fio condutor ao longo
das quatro sessões do curso de Engenharia de Dados FAIR. Os participantes trabalham o
**mesmo** conjunto do dia 1 ao dia 4: avaliam-no de forma intuitiva, modelam os dados,
modelam os metadados, publicam e avaliam a FAIRness — vendo o objeto sair de “nada FAIR”
para “FAIR”.

> **Tema:** pratos e culinárias do mundo. Escolhido por ser compreensível por qualquer
> público, independentemente de área ou nacionalidade, e por ser culturalmente inclusivo
> (pratos de ~24 países, incluindo o Brasil).

## Por que é seguro quanto a direitos autorais

- Nomes de pratos, países e ingredientes são **fatos de conhecimento comum** — não são
  protegidos por direito autoral.
- Os valores numéricos (calorias, tempo de preparo, ano, nível de picância) são
  **ilustrativos e inventados** para fins didáticos — **não** foram copiados de nenhuma
  base de dados. Não devem ser citados como dados reais.
- Resultado: o conjunto pode ser distribuído, modificado e republicado livremente no curso.

## Arquivos

| Arquivo | O que é |
|---|---|
| `dishes.csv` | Tabela principal — 55 pratos. **Propositalmente “suja” e sem metadados** (estado de chegada, não-FAIR). |
| `countries.csv` | Tabela de apoio — 24 países com nome canônico, ISO3, região e QID do Wikidata. Para ensinar ligação/reconciliação. |
| `GUIA_Exemplo_Condutor.md` | Este guia (somente para o instrutor — não distribuir aos alunos no início). |

## Dicionário de dados de `dishes.csv`

Cabeçalhos **propositalmente crípticos/ambíguos** (parte do exercício é interpretá-los):

| Coluna | Pretende significar | Observações |
|---|---|---|
| `id` | Identificador local (D001…) | Local, **não é um PID** — ponto de ensino (F1). |
| `dish` | Nome do prato | — |
| `ctry_origin` | País de origem | Grafias inconsistentes (ver abaixo). |
| `region` | Continente/região | Valores inconsistentes e faltantes. |
| `main_ingredients` | Ingredientes principais | **Múltiplos valores numa célula**, separadores variados (`;`, `,`, `/`). |
| `course` | Tipo (entrada/principal/sobremesa/lanche) | Rótulos inconsistentes. |
| `veg` | Vegetariano? | Codificação booleana mista. |
| `kcal` | Calorias (por porção) | **Unidade não declarada**; faltantes; separadores mistos. |
| `prep_time` | Tempo de preparo | Formatos mistos (`20 min`, `45`, `1h`, `1h30`, `unknown`). |
| `year` | Ano/época de origem | Formatos mistos e imprecisos. |
| `spice` | Picância (0–5) | **Tipo misto** (números e texto `mild`/`hot`); faltantes. |

## Problemas FAIR/qualidade embutidos (de propósito)

Use esta tabela para guiar a descoberta pelos alunos. Cada problema conecta-se a um
princípio FAIR e à sessão que o trata.

| # | Problema | Exemplo no dado | Princípio | Tratado na |
|---|---|---|---|---|
| 1 | Sem identificador persistente | `id = D001` (local) | F1 | S1 (diagnóstico), S4 (DOI) |
| 2 | Sem metadados / sem dicionário | (nenhum arquivo de metadados) | F2, F3, R1 | S3 (Contour), S4 |
| 3 | Sem licença | (ausente) | R1.1 | S4 |
| 4 | Sem proveniência | (ausente: quem, quando, fonte) | R1.2 | S3/S4 |
| 5 | Grafias inconsistentes de país | `Brasil`/`Brazil`, `USA`/`United States`/`U.S.A.`, `UK`, `Korea`/`South Korea`, `Mexico`/`México` | I (reconciliação) | S2 (OpenRefine) |
| 6 | Múltiplos valores numa célula | `main_ingredients` com `;`, `,`, `/` | I1, modelagem | S2 |
| 7 | Codificação booleana mista | `veg` = `yes/no/Y/N/sim/não` | R1, I | S2 |
| 8 | Unidade não declarada / mista | `kcal` sem unidade; `1,200` vs `1.200` | I, R1 | S2 |
| 9 | Formatos de data/tempo mistos | `year`, `prep_time` | I1 | S2 |
| 10 | Valores faltantes inconsistentes | em branco, `NA`, `-`, `unknown` | R1 | S2 |
| 11 | Rótulos categóricos inconsistentes | `course` = `main`/`Main course`/`maindish` | I | S2 |
| 12 | Sem vocabulário/semântica | colunas como texto livre, sem URIs | I1, I2, I3 | S2 (modelo), S3 (metadados) |
| 13 | Não indexado/descobrível | arquivo solto, sem registro | F4 | S4 |

## Modelo semântico-alvo (para a Sessão 2)

Um modelo simples e reutilizável para a “FAIRificação” dos **dados**:

- Classes: `:Dish`, `schema:Country`, `:Ingredient`.
- Relações sugeridas (reusando vocabulários existentes onde possível):
  - `:Dish :originatesIn schema:Country`
  - `:Dish :hasMainIngredient :Ingredient`
  - `:Dish :courseType` → valor de um vocabulário controlado (entrada/principal/sobremesa/lanche)
  - `:Dish schema:isVegetarian xsd:boolean`
- Reconciliar `ctry_origin` → Wikidata (coluna `wikidata_qid` em `countries.csv` ajuda a conferir),
  e ingredientes → Wikidata, no OpenRefine.

Exemplo (Turtle) de um prato já “linkável”:

```turtle
@prefix :      <https://example.org/dishes/> .
@prefix schema:<http://schema.org/> .
@prefix wd:    <http://www.wikidata.org/entity/> .
@prefix xsd:   <http://www.w3.org/2001/XMLSchema#> .

:D001 a :Dish ;
    schema:name "Feijoada" ;
    :originatesIn wd:Q155 ;            # Brazil
    :hasMainIngredient wd:Q3127593 ;  # black beans (exemplo)
    :courseType :MainCourse ;
    schema:isVegetarian false .
```

## Esquema de metadados-alvo (para a Sessão 3 — Contour)

Na Sessão 3, os participantes constroem no **Contour** um esquema SHACL para descrever o
**conjunto** (`dcat:Dataset`): `dct:title`, `dct:description`, `dct:creator`/`dct:publisher`
(`foaf:Agent`), `dct:issued`, `dct:license`, `dcat:theme`. É exatamente o que falta hoje
(problemas #2, #3, #4).

## Recurso FAIR-alvo (para a Sessão 4)

Estado “bom” a alcançar e comparar com a autoavaliação intuitiva do dia 1:

```turtle
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dct:  <http://purl.org/dc/terms/> .

<https://doi.org/10.5281/zenodo.XXThis identifier is illustrativeXX> a dcat:Dataset ;
    dct:title "World Dishes — exemplo didático FAIR"@pt ;
    dct:description "Conjunto sintético de pratos do mundo para o curso FAIR."@pt ;
    dct:creator <https://orcid.org/0000-0000-0000-0000> ;
    dct:publisher <https://ror.org/...> ;
    dct:issued "2026-06-22"^^xsd:date ;
    dct:license <https://creativecommons.org/licenses/by/4.0/> ;
    dcat:theme <http://www.wikidata.org/entity/Q746549> .   # dish
```

(Identificadores acima são **ilustrativos**; obter DOI/ORCID/ROR reais é parte da S4.)

## Uso por sessão (o fio condutor)

1. **Sessão 1 — Princípios.** Distribuir `dishes.csv` (e `countries.csv`) sem explicação.
   Prática: avaliação **intuitiva** de FAIRness — os alunos listam o que está errado
   (problemas #1–#13). Gera a “linha de base”.
2. **Sessão 2 — Semântica + modelagem de dados.** No OpenRefine: limpar (#5–#11),
   reconciliar países/ingredientes ao Wikidata, definir o modelo semântico e exportar RDF.
3. **Sessão 3 — Metadados.** No Contour: modelar o esquema de metadados do conjunto
   (`dcat:Dataset`), gerar o SHACL, salvar `.ttl`.
4. **Sessão 4 — Publicação + avaliação.** Publicar (sandbox/FAIR Data Point) com metadados,
   licença e PID; avaliar com F-UJI/FAIR Champion e **comparar** com a avaliação intuitiva do dia 1.

> Sugestão didática: manter visível, ao longo dos dias, um “placar FAIR” do conjunto —
> de quase tudo vermelho (dia 1) a majoritariamente verde (dia 4).
