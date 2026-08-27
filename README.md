# JABUTI-SQL

**J**oint **A**nnotated **B**enchmark for **U**ser-centric **T**ext-to-SQL **I**n Real Health Data

Benchmark brasileiro para avaliação de sistemas *Text-to-SQL*, construído a partir da integração de cinco bases públicas reais do DATASUS (Banco de Preços em Saúde, Hospitais e Leitos, BNAFAR, CNES e divisões de Macrorregião/Região de Saúde). Diferente de benchmarks sintéticos como Spider e BIRD, o JABUTI-SQL preserva características de ambientes governamentais reais, dados incompletos, ambiguidades semânticas e relações complexas entre múltiplas fontes.

📄 Paper: *[link do paper SBBD/DSW 2026]*
📦 Dados completos: [Hugging Face — BotelhoMS/JABUTI-SQL](https://huggingface.co/datasets/BotelhoMS/JABUTI-SQL)
🗄️ DOI (Zenodo): [10.5281/zenodo.21048827](https://doi.org/10.5281/zenodo.21048827)
📜 Licença: CC BY 4.0 (ver [`LICENSE`](./LICENSE))

---

## Sumário

- [Visão geral](#visão-geral)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Como restaurar o banco](#como-restaurar-o-banco)
- [Esquema relacional e dicionário de dados](#esquema-relacional-e-dicionário-de-dados)
- [Pares pergunta/SQL](#pares-perguntasql)
- [Limitações conhecidas](#limitações-conhecidas)
- [Como citar](#como-citar)

---

## Visão geral

O benchmark combina três domínios principais, gastos públicos e compras (BPS), infraestrutura hospitalar (Hospitais e Leitos) e assistência farmacêutica (BNAFAR), enriquecidos com dados geográficos e administrativos (CNES, Macrorregião/Região de Saúde). O recorte temporal é o ano de 2025.

O conjunto inclui **69 pares pergunta/SQL** em português, anotados manualmente, distribuídos em:

| Dimensão | Categorias |
|---|---|
| Persona | Usuário leigo · Médico gestor hospitalar · Analista de dados de saúde (23 perguntas cada) |
| Dificuldade | Fácil (30, ≈44%) · Médio (30, ≈43%) · Difícil (18, ≈26%) |
| Tema | Geográfica, Leitos, Estoque, Financeira, Produtos, Instituições de saúde, Caracterização dos dados |

## Estrutura do repositório

```
.
├── ddl/                     # scripts de criação do esquema relacional (15 tabelas)
├── docs/
│   ├── dicionario_dados_Jabuti.pdf   # dicionário de dados completo 
├── questoes/                # pares pergunta/SQL, com persona, dificuldade e categoria temática
├── CITATION.cff             
└── README.md
```


## Como restaurar o banco

**Pré-requisitos:** PostgreSQL instalado.

```bash
# 1. Baixe o dump completo do Hugging Face
#    https://huggingface.co/datasets/BotelhoMS/JABUTI-SQL

# 2. Crie o banco de dados
psql -U postgres -c "CREATE DATABASE jabuti_sql;"

# 3. Restaure o dump
psql -U postgres -d jabuti_sql < dump.sql
```

## Esquema relacional e dicionário de dados

15 tabelas, entre 2 e 33 colunas, de 5 registros (Região do Brasil) a mais de 143 milhões (Instituição Estoca Produto):

| Tabela | Colunas | Linhas |
|---|---|---|
| Instituição | 33 | 610.321 |
| Mantenedora | 4 | 7.390 |
| Endereço | 9 | 442.672 |
| Município | 5 | 5.570 |
| Região de Saúde | 3 | 439 |
| Macrorregião de Saúde | 3 | 121 |
| Unidade Federativa | 4 | 27 |
| Região do Brasil | 2 | 5 |
| Leitos | 17 | 86.147 |
| Fornecedor | 3 | 1.083 |
| Fabricante | 3 | 751 |
| Produto | 5 | 20.875 |
| CATMAT | 3 | 16.953 |
| Mantenedora Compra Produto | 14 | 26.215 |
| Instituição Estoca Produto | 11 | 143.009.579 |


## Pares pergunta/SQL

Exemplo (nível fácil):

> **Pergunta:** Por tipo de gestão, quantas instituições de saúde estão ativas? Liste primeiro os tipos com mais unidades.

```sql
SELECT tipo_de_gestao, COUNT(*) AS total_instituicoes
FROM instituicao
WHERE motivo_da_desabilitacao IS NULL
GROUP BY tipo_de_gestao
ORDER BY total_instituicoes DESC
```

Mais exemplos (médio e difícil) e a lista completa estão em [`questoes/`](./questoes).

## Limitações conhecidas

- **Valores nulos**: até 44,48% em `instituicao_estoca_produto`, 28,96% em `instituicao`, 13,27% em `endereco`. Preservados intencionalmente para refletir a qualidade real dos dados do SUS. Detalhes por tabela em [`docs/data-dictionary.md`](./docs/data-dictionary.md).
- **Registros sem região associada**: 47,66% dos registros de estoque não têm vínculo geográfico, por dependerem do preenchimento consistente do código CNES pelas instituições.
- **Escala**: 69 pares é modesto frente a Spider (8.659) e BIRD (12.751) — o benchmark prioriza realismo e representatividade sobre volume.

## Como citar

O GitHub gera a citação automaticamente a partir do [`CITATION.cff`](./CITATION.cff) — veja o botão **"Cite this repository"** na barra lateral do repositório. BibTeX:

```bibtex
@inproceedings{botelho2026jabuti,
  title     = {JABUTI-SQL: Um Benchmark em Português para Avaliação de Abordagens Text-to-SQL em Cenários Reais},
  author    = {Botelho, Matheus and Fonseca, Guilherme and Silva, Lucas R. and Gonçalves, Rodrigo M. and Lobato, Yago and Ribeiro, Gustavo A. and Camara, Eduardo C. and Zardin, Jaide F. C. and Calais, Pedro and Simões, Emerson A. and Sene, Allan and Reis, Julio C. S. and da Silva, Altigran and Gonçalves, Marcos André},
  booktitle = {Anais do Simpósio Brasileiro de Banco de Dados (SBBD) — Dataset Showcase Workshop},
  year      = {2026}
}
```
