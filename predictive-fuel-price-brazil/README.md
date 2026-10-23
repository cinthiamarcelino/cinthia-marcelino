# 📈 Modelo Preditivo de Preços: Gasolina e Etanol vs Inflação (IPCA)

## 🎯 Sobre o Projeto
Este projeto tem como objetivo analisar a série histórica dos preços de venda da Gasolina e do Etanol no Brasil (2004 a 2021) e criar modelos preditivos capazes de prever o valor de venda destes combustíveis, verificando também a sua correlação com o IPCA (termômetro oficial da inflação no Brasil)[cite: 8].

O projeto cobre o ciclo completo de vida dos dados (ETL, Análise Exploratória e Machine Learning, Deep Learning), processando dados públicos da Agência Nacional do Petróleo (ANP) e do IBGE[cite: 5, 9].

---

## 🏗️ Arquitetura e Pipeline de Dados

**1. Coleta e Processamento (ETL):**
Utilização do **KNIME Analytics Platform** para a leitura, limpeza e consolidação de 72 arquivos CSV originais contendo a série histórica de combustíveis[cite: 8, 10, 11]. O pipeline também contemplou o cruzamento com dados do IBGE, normalização dos valores (trazendo a mesma base de preços para comparação justa no tempo) e agrupamento mensal/anual[cite: 14, 15].

**2. Análise Exploratória de Dados (EDA):**
Análise gráfica utilizando Python (`pandas`, `matplotlib`, `plotly`) para investigar a evolução do preço médio e demonstrar como aumentos (especialmente em 2021) descolaram do índice de inflação (IPCA acumulado de 12 meses)[cite: 17, 18, 35].

**3. Modelagem Preditiva (Machine Learning):**
Os dados foram divididos em 70% para treino e 30% para teste, passando por normalização (`MinMaxScaler`)[cite: 8, 21]. Foram desenvolvidos e comparados dois modelos:
* **Ridge Regression (Scikit-Learn):** Método de regularização (com ajuste de hiperparâmetro alpha=0.1) para suavizar atributos e reduzir ruídos[cite: 21, 24].
* **Redes Neurais LSTM - Long Short-Term Memory (Keras/TensorFlow):** Arquitetura de Deep Learning estruturada com 1 camada de entrada, 3 camadas LSTM intercaladas com 2 camadas de Dropout (para evitar *overfitting*) e 1 camada Densa de saída[cite: 25, 27].

---

## 🏆 Resultados e Performance dos Modelos

A avaliação dos modelos utilizou as métricas MSE (Mean Squared Error), MAE e RMSE[cite: 31]. Ambos os modelos tiveram resultados altamente satisfatórios, identificando com precisão a tendência de valores reais[cite: 32]:

* **Cenário Gasolina:** O modelo de Redes Neurais **LSTM** obteve a melhor performance, atingindo o menor MSE (**0,00139**)[cite: 31].
* **Cenário Etanol:** O modelo **Ridge Regression** obteve o melhor desempenho, atingindo o menor MSE (**0,00216**)[cite: 31, 32].

---

## 🛠️ Stack Tecnológico
* **Engenharia de Dados (ETL):** KNIME Analytics Platform[cite: 10]
* **Linguagem:** Python[cite: 17]
* **Machine Learning / Deep Learning:** Scikit-Learn, Keras, TensorFlow[cite: 21, 25]
* **Bibliotecas Analíticas:** Pandas, NumPy, MinMaxScaler[cite: 21, 35, 42]