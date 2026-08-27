# Contribuindo com o JABUTI-SQL

Obrigado pelo interesse em contribuir! Formas de ajudar:

## Submeter um resultado para o leaderboard

1. Rode seu modelo sobre o conjunto completo de 69 pares em [`questoes/benchmark.json`](./questoes/benchmark.json).
2. Abra uma *issue* ou *pull request* com:
   - Nome e versão do modelo
   - Acurácia de execução (EX) obtida
   - Link para o código/prompt usado na avaliação
   - Confirmação de que o modelo **não** foi treinado ou fine-tunado sobre o JABUTI-SQL (o benchmark não tem split treino/teste — treinar no conjunto invalida a comparação)
3. Resultados sem código/prompt reprodutível não serão aceitos no leaderboard.

## Reportar problemas nos dados

Encontrou uma inconsistência, um par pergunta/SQL incorreto, ou uma lacuna na documentação (`docs/dicionario_dados_Jabuti.pdf`)? Abra uma *issue* descrevendo:

- Qual tabela/coluna ou qual par pergunta/SQL
- O problema encontrado
- Se possível, a query ou trecho que evidencia o problema

## Propor novos pares pergunta/SQL

Novas contribuições de pares pergunta/SQL são bem-vindas, desde que:

- Sigam o esquema relacional existente (ver `docs/dicionario_dados_Jabuti.pdf`)
- Especifiquem persona (usuário leigo, médico gestor hospitalar ou analista de dados) e nível de dificuldade (fácil, médio, difícil)
- Venham com a query SQL validada contra o dump oficial

## Dúvidas

Abra uma *issue* com a tag `question`, ou entre em contato pelos e-mails listados no paper.
