# Credit Default Prediction — LendingClub

### Application Score & Credit Risk Modeling

Projeto de **Machine Learning aplicado a Credit Risk**, utilizando dados históricos do LendingClub (2007–2010) para estimar o risco de default e simular o desenvolvimento de um **Application Score para apoio à decisão de crédito**.

O projeto compara modelos lineares e não lineares, considerando **desbalanceamento de classes, seleção de atributos, controle de data leakage, Cross-Validation, otimização de threshold e métricas específicas de risco de crédito**.

---

## 🎯 Problema de Negócio

Em uma operação de crédito, uma decisão incorreta pode gerar impactos financeiros relevantes:

- **aprovar um cliente inadimplente** → potencial perda financeira;
- **recusar um bom cliente** → perda de receita e oportunidade de negócio.

O objetivo deste projeto foi desenvolver modelos capazes de identificar clientes com maior probabilidade de **não quitar integralmente seus empréstimos**, permitindo analisar diferentes estratégias de decisão conforme o nível de risco aceitável.

A variável-alvo utilizada foi:

**`not.fully.paid`**

- `0` → empréstimo totalmente pago
- `1` → empréstimo não totalmente pago

---

## 📊 Principais Resultados

A avaliação foi realizada utilizando **Cross-Validation estratificada** e uma avaliação final em **dados de teste não utilizados no treinamento**.

### Cross-Validation

| Modelo | ROC-AUC | PR-AUC |
| --- | --- | --- |
| **Logistic Regression** | **0,677 ± 0,005** | **0,292 ± 0,020** |
| Random Forest | 0,668 ± 0,006 | 0,282 ± 0,024 |
| XGBoost | 0,667 ± 0,012 | 0,285 ± 0,024 |

A **Logistic Regression apresentou o melhor desempenho médio na Cross-Validation**, enquanto os modelos baseados em árvores apresentaram desempenho competitivo.

### Métrica KS (Kolmogorov-Smirnov) — Conjunto de Teste

| Modelo | KS | Threshold no KS | TPR | FPR |
| --- | --- | --- | --- | --- |
| Logistic Regression | 24,5% | 0,153 | 74,3% | 49,7% |
| **Random Forest** | **27,4%** | 0,322 | **77,2%** | 49,8% |

No conjunto de teste, o **Random Forest apresentou maior capacidade de discriminação**, alcançando KS de **27,4%**, contra **24,5%** da Logistic Regression.

Esse resultado não significa que o Random Forest seja superior em todas as métricas. A comparação entre os modelos considera conjuntamente **performance, estabilidade, interpretabilidade e objetivo de negócio**.

---

## 🏗️ Pipeline de Machine Learning

A solução foi estruturada como um pipeline de modelagem seguindo as principais etapas do **CRISP-DM**:

```
Raw Data
    ↓
Data Understanding / EDA
    ↓
Data Preparation
    ↓
Feature Engineering
    ↓
WoE / IV / L1 / VIF
    ↓
Train / Cross-Validation
    ↓
Model Comparison
    ↓
Threshold Optimization
    ↓
Final Test Evaluation
    ↓
Credit Risk Analysis
```

### Principais etapas

- análise exploratória dos dados;
- tratamento e preparação das variáveis;
- análise de desbalanceamento;
- Weight of Evidence (WoE);
- Information Value (IV);
- seleção de atributos com L1;
- análise de multicolinearidade com VIF;
- treinamento de diferentes algoritmos;
- Stratified Cross-Validation;
- otimização de hiperparâmetros;
- análise de diferentes thresholds;
- avaliação final no conjunto de teste;
- análise de importância das variáveis.

---

## 📐 Metodologia — CRISP-DM

O projeto foi desenvolvido seguindo as principais etapas do CRISP-DM:

**Business Understanding**

Definição do problema de risco de crédito e dos impactos de falsos positivos e falsos negativos.

**Data Understanding**

Exploração da base LendingClub, distribuição das variáveis e análise da variável-alvo.

**Data Preparation**

Limpeza, preparação, transformação e seleção dos atributos.

**Modeling**

Treinamento e comparação de diferentes modelos de classificação.

**Evaluation**

Avaliação utilizando métricas de discriminação, classificação e risco de crédito.

---

## 📊 Análise Exploratória

A EDA buscou identificar padrões relevantes antes da modelagem.

### Distribuição dos empréstimos por classe

### FICO Score × Default

### Principais insights

- Clientes com **FICO Score mais baixo** apresentaram maior concentração de defaults.
- **Taxas de juros mais elevadas** estiveram associadas a maior incidência de inadimplência.
- A variável-alvo apresenta **desbalanceamento significativo**, tornando Accuracy isoladamente inadequada para avaliar o desempenho dos modelos.

---

## 🧩 Engenharia de Atributos

Uma das preocupações centrais do projeto foi evitar **data leakage** durante a preparação dos dados e seleção das variáveis.

Foram utilizadas técnicas como:

- padronização de variáveis numéricas;
- tratamento de variáveis categóricas;
- divisão estratificada dos dados;
- **Weight of Evidence (WoE)**;
- **Information Value (IV)**;
- seleção por **L1 / Lasso**;
- remoção de multicolinearidade utilizando **VIF**.

> **A seleção de atributos e transformações utilizadas durante o desenvolvimento foram realizadas exclusivamente sobre o conjunto de treino, evitando que informações do conjunto de teste influenciassem o modelo.**
> 

---

## 🤖 Modelagem

Foram avaliados quatro algoritmos:

| Modelo | Papel na análise |
| --- | --- |
| **Logistic Regression** | Baseline interpretável |
| Decision Tree | Benchmark inicial |
| Random Forest | Ensemble não linear |
| XGBoost | Modelo de boosting |

O modelo de **Logistic Regression** foi utilizado como referência por sua interpretabilidade e relevância histórica em aplicações de credit scoring.

O **Random Forest** foi utilizado como principal benchmark não linear, permitindo investigar relações e interações que podem não ser capturadas por um modelo linear.

O **XGBoost** apresentou desempenho competitivo, mas optou-se por não aprofundar sua otimização devido ao maior espaço de hiperparâmetros e custo computacional, mantendo o foco em uma comparação equilibrada entre um modelo linear e um ensemble.

### Hiperparametrização

- Randomized Search
- Stratified Cross-Validation
- 5 folds
- `scale_pos_weight` no XGBoost para tratamento do desbalanceamento

### Por que o Decision Tree não foi aprofundado?

O modelo Decision Tree apresentou desempenho inferior aos modelos mais competitivos e maior tendência ao overfitting.

Por isso, as análises posteriores foram concentradas principalmente em **Logistic Regression, Random Forest e XGBoost**.

---

## 📈 Comparação dos Modelos

### ROC-AUC — Cross-Validation

A Cross-Validation mostrou que a **Logistic Regression apresentou o melhor desempenho médio**, enquanto Random Forest e XGBoost permaneceram competitivos.

Esse resultado é importante porque demonstra que maior complexidade do modelo não implica necessariamente melhor capacidade de generalização.

A análise posterior considerou também métricas específicas de crédito e o comportamento dos modelos no conjunto de teste.

---

## 🎚️ Threshold Optimization

O threshold padrão de **0,50** não foi assumido como necessariamente ideal.

Foram avaliados diferentes pontos de corte considerando o trade-off entre:

- Recall;
- Precision;
- Specificity;
- F1-Score.

| Modelo | Threshold | F1-Score |
| --- | --- | --- |
| Logistic Regression | **0,15** | **0,350** |
| Random Forest | **0,45** | **0,341** |
| XGBoost | 0,15 | 0,339 |

### Insight de negócio

O threshold representa uma decisão de negócio:

**Threshold menor**

→ maior capacidade de capturar potenciais inadimplentes

→ maior número de falsos positivos

**Threshold maior**

→ menor número de falsos positivos

→ maior risco de deixar inadimplentes passarem

Em uma aplicação real, o threshold deve ser definido considerando os **custos financeiros associados a falsos positivos e falsos negativos**, e não apenas uma métrica estatística.

---

# 📏 Avaliação Final

Os modelos finais selecionados foram avaliados utilizando:

- ROC-AUC;
- PR-AUC;
- Precision;
- Recall;
- Specificity;
- F1-Score;
- Matriz de Confusão;
- Kolmogorov-Smirnov (KS).

### Curva ROC

### Matriz de Confusão

---

## 📐 Análise da Curva KS

A curva **Kolmogorov-Smirnov (KS)** foi utilizada para avaliar a capacidade dos modelos em discriminar clientes que apresentaram default daqueles que não apresentaram.

O KS corresponde à maior diferença entre a **Taxa de Verdadeiros Positivos (TPR)** e a **Taxa de Falsos Positivos (FPR)**.

| Modelo | KS | Threshold no KS | TPR | FPR |
| --- | --- | --- | --- | --- |
| Logistic Regression | 24,5% | 0,153 | 74,3% | 49,7% |
| **Random Forest** | **27,4%** | 0,322 | **77,2%** | 49,8% |

O **Random Forest apresentou o maior KS (27,4%)**, superando a Logistic Regression (24,5%). No ponto de máximo KS, também apresentou maior TPR, enquanto o FPR permaneceu praticamente equivalente.

Como referência heurística frequentemente utilizada em credit scoring, valores entre **20% e 30%** podem ser interpretados como capacidade de discriminação moderada. Essas faixas são referências práticas e não representam um padrão regulatório universal.

É importante destacar que os valores apresentados foram calculados **no conjunto de teste**, separado durante o desenvolvimento dos modelos. Dessa forma, representam uma avaliação **out-of-sample** da capacidade de generalização e podem ser inferiores aos resultados observados durante o desenvolvimento.

Apesar da vantagem do Random Forest no KS, a diferença foi moderada. A Logistic Regression permaneceu competitiva e apresenta uma vantagem importante em aplicações de crédito: **maior interpretabilidade**.

---

## 🎯 Conclusão

Este projeto mostrou que a escolha de um modelo de **Credit Risk** não deve ser baseada em uma única métrica.

A **Logistic Regression apresentou o melhor desempenho médio na Cross-Validation**, enquanto o **Random Forest apresentou o maior KS na avaliação final do conjunto de teste**.

Essa diferença evidencia um importante trade-off entre:

**Performance preditiva × Interpretabilidade × Estratégia de decisão**

Além da comparação entre algoritmos, o projeto buscou reproduzir um fluxo de modelagem próximo de um cenário real de crédito, incluindo:

**Data Leakage Control → Feature Selection → Cross-Validation → Model Comparison → Threshold Optimization → Out-of-Sample Evaluation**

O resultado mais importante não é apenas qual modelo apresentou a maior métrica, mas **como diferentes modelos e thresholds podem apoiar diferentes estratégias de risco e decisão de crédito**.

---

## 🛠️ Tecnologias

---

**Python · Pandas · NumPy · Scikit-learn · XGBoost · Matplotlib · Seaborn · Jupyter Notebook**

---

## 👤 Sobre o Projeto

Projeto desenvolvido como parte do meu portfólio de **Data Science**, com foco em aplicações de **Machine Learning para Credit Risk, modelagem preditiva e tomada de decisão baseada em dados**.

### Conecte-se comigo

- 🌎 Portfolio
- 💼 LinkedIn
- 💻 GitHub
- 📧 Email
