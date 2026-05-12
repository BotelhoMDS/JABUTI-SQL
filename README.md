# JABUTI-SQL

**J**oint **A**nnotated **B**enchmark for **U**ser-centric **T**ext-to-SQL **I**n Real Health Data

Benchmark brasileiro para avaliação de sistemas Text-to-SQL construído a partir de dados públicos reais do DATASUS, integrando múltiplas fontes heterogêneas relacionadas à assistência farmacêutica, compras públicas e infraestrutura hospitalar do Sistema Único de Saúde (SUS).

---

## 📦 Dados

O dump completo do banco de dados está hospedado no Hugging Face:

🔗 **[https://huggingface.co/datasets/BotelhoMS/JABUTI-SQL](https://huggingface.co/datasets/BotelhoMS/JABUTI-SQL)**

### Como restaurar o banco

**Pré-requisitos:** PostgreSQL instalado

```bash
# 1. Crie o banco de dados
psql -U postgres -c "CREATE DATABASE jabuti_sql;"

# 2. Restaure o dump
psql -U postgres -d jabuti_sql < dump.sql
```
