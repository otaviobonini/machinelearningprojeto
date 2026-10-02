# Previsão de inadimplência e valor em risco no cartão de crédito

Projeto de Machine Learning Clássico, Tema 4.5: Default Credit Card.

**Alunos:** Otávio Bonini e Cauã Medeiros
**Professor:** Rodrigo Ramos Silva

## O que o projeto faz

O projeto responde a duas perguntas sobre cada cliente de cartão de crédito:

1. **Classificação:** qual a chance de o cliente dar calote no próximo mês?
2. **Regressão:** quanto o cliente deve (valor da fatura)?

Multiplicando as duas respostas, cada cliente ganha um **valor em risco**. Ordenando os clientes por esse valor, o banco tem uma fila de cobrança: começa por quem tem mais dinheiro em jogo.

## Arquivos

| Arquivo | O que é |
|---|---|
| `Projeto_ML_Credito.ipynb` | Notebook do Google Colab com o projeto inteiro, do carregamento dos dados até o valor em risco |
| `Projeto_ML_Credito.pdf` | Documento do projeto (relatório em formato ABNT) |
| `requirements.txt` | Bibliotecas usadas |

## Base de dados

**Default of Credit Card Clients**, do UCI Machine Learning Repository (YEH; LIEN, 2009).
30.000 clientes de um banco de Taiwan, com 6 meses de histórico (abril a setembro de 2005). Cerca de 22% deram calote.

A base é baixada automaticamente dentro do notebook pelo pacote `ucimlrepo` (id 350). Não é preciso baixar nada antes.

Link: https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients

## Como rodar

1. Abra o arquivo `Projeto_ML_Credito.ipynb` no Google Colab (no GitHub, clique no arquivo e depois em "Open in Colab", ou faça upload em https://colab.research.google.com).
2. No menu, clique em **Ambiente de execução > Executar tudo**.
3. A primeira célula instala o `ucimlrepo` e a segunda baixa a base. O resto roda em poucos minutos.

## Especificações técnicas

- **Linguagem:** Python 3
- **Ambiente:** Google Colab (não precisa de GPU)
- **Bibliotecas:** pandas, numpy, scikit-learn, matplotlib, seaborn, ucimlrepo (lista em `requirements.txt`)
- **Reprodutibilidade:** todos os sorteios usam `random_state=42`, então o resultado é sempre o mesmo

Para rodar fora do Colab:

```
pip install -r requirements.txt
```

## Etapas do notebook

1. Carregamento da base e renomeação das colunas
2. Análise exploratória: taxa de calote, nulos, duplicadas, códigos inválidos
3. Pré-processamento: limpeza (399 linhas com códigos que não existem na documentação), 4 colunas novas, divisão treino/teste (80/20, estratificada), StandardScaler e One-Hot Encoder dentro de um Pipeline
4. Classificação: Regressão Logística e Random Forest, ajustados com GridSearchCV (validação cruzada em 5 partes, critério F1)
5. Regressão: Linear, Polinomial e Random Forest Regressor, prevendo a fatura de setembro sem usar nenhuma coluna de setembro
6. Importância das variáveis e valor em risco

## Principais resultados (conjunto de teste, 5.921 clientes)

| Modelo de classificação | Acurácia | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| **Random Forest** | 76,9% | 48,5% | 59,4% | 0,534 | 0,780 |
| Regressão Logística | 74,3% | 44,6% | 62,7% | 0,521 | 0,761 |

| Modelo de regressão | Erro médio (MAE) | R² |
|---|---|---|
| **Random Forest Regressor** | NT$ 7.566 | 0,920 |
| Regressão Linear | NT$ 8.548 | 0,910 |
| Polinomial (grau 2) | NT$ 8.787 | 0,913 |

**Valor em risco:** entre os 10% de clientes do topo da fila, 40,7% deram calote, contra 22,3% na carteira toda (1,82 vez mais).

## Referência da base

YEH, I-Cheng; LIEN, Che-hui. The comparisons of data mining techniques for the predictive accuracy of probability of default of credit card clients. **Expert Systems with Applications**, v. 36, n. 2, p. 2473-2480, 2009.
