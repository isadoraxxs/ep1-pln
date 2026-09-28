# Progresso EP1

## Dataset

- 20.092 instâncias
- Classes:
  - c1: 6.347
  - c234: 6.853
  - c5: 6.892
- 18.432 textos únicos
- 1.660 ocorrências duplicadas adicionais
- 291 textos únicos possuem labels conflitantes

## Estratégia de validação

- Holdout externo interno ao train.xlsx:
  - Treino: 16.073
  - Validação: 4.019
- StratifiedGroupKFold usando textos idênticos como grupos
- Holdout de 4.019 ainda não utilizado para seleção dos modelos

## Resultados

### Baselines
- Classe majoritária: 0.3431
- TF-IDF + Logistic Regression inicial: 0.4645 no holdout

### Cross-validation no conjunto de desenvolvimento

- Word TF-IDF + Logistic Regression: ~0.4481
- Character TF-IDF + Logistic Regression: ~0.4438
- Word + Char + Logistic Regression: ~0.4496
- Word + Char + LinearSVC: ~0.4498

### Melhor configuração atual

Word + Character TF-IDF:
- Word ngram_range: (1, 3)
- Character ngram_range: (3, 5)
- min_df: 2
- max_features: 150.000 em cada ramo
- SelectKBest chi2: k=150.000
- LinearSVC C=0.05

Confirmação com StratifiedGroupKFold de 5 folds:
- CV accuracy: 0.454738
- CV std: 0.007643
- Train accuracy: 0.675449

### Experimentos que não melhoraram

Features estruturais:
- CV accuracy: 0.454428

Remoção de textos com labels conflitantes:
- CV accuracy: 0.451937

Deduplicação + label majoritário:
- CV accuracy: 0.452997

## Próximos passos

1. Encerrar investigação de duplicatas.
2. Testar representação distribuída / embeddings.
3. Possivelmente:
   - média de embeddings
   - média ponderada por TF-IDF
   - embeddings pré-treinados vs. treinados no corpus
4. Depois testar BERTimbau.
5. Somente após congelar modelos usar o holdout externo de 4.019 exemplos.