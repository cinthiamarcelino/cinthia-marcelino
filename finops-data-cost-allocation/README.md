# 📊 Motor Analítico de Data FinOps — Rateio e Governança de Custos de Dados

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas)
![Data FinOps](https://img.shields.io/badge/Domain-Data%20FinOps%20%26%20Chargeback-green?style=for-the-badge)
![Data Governance](https://img.shields.io/badge/Governance-Data%20Quality%20%26%20Orphans-orange?style=for-the-badge)

---

## 🎯 Sobre o Projeto

No cenário corporativo moderno, o consumo de APIs e fontes externas de dados (Bureaus de Crédito, Banco Central, Receita Federal, etc.) representa um dos custos operacionais mais voláteis e difíceis de controlar. Sem uma arquitetura clara de **Data FinOps**, a organização enfrenta opacidade de custos, impossibilidade de auditoria e falhas na alocação orçamentária.

Este projeto entrega um **Produto de Dados (Data Product)** completo: um **Motor Analítico de Rateio de Custos de Dados (Chargeback/Showback)** desenvolvido em Python/Pandas. O motor unifica a ingestão de logs brutos de consumo, cruza com regras relacionais de RH, liderança e catálogo de fornecedores, e gera visões executivas prontas para tomada de decisão e auditoria financeira.

---

## 🏗️ Arquitetura e Modelagem de Dados

O motor segue o modelo relacional de **Star Schema / Dimensional Modeling**, onde a tabela fato armazena as transações puras e as dimensões enriquecem a informação técnica e de negócio.

```
                  ┌──────────────────────┐
                  │  usuarios_areas.csv  │
                  │  (Dimensão Usuário)  │
                  └──────────┬───────────┘
                             │
                             ▼
┌──────────────────┐      ┌──────────────────────┐      ┌────────────────────────┐
│catalogo_precos.csv│ ───► │  consumo_dados.csv   │ ◄─── │  areas_lideranca.csv   │
│(Dimensão Preço)  │      │    (Tabela Fato)     │      │ (Dim. Áreas/Governança)│
└──────────────────┘      └──────────────────────┘      └────────────────────────┘
```

### 📁 Estrutura das Bases de Entrada (`/arquivos`)

1. **`consumo_dados.csv` (Fato):** Transações de requisições de dados por usuário, produto e data.
2. **`usuarios_areas.csv` (Dimensão):** Mapeamento do usuário para sua respectiva área.
3. **`areas_lideranca.csv` (Dimensão):** Mapeamento da área, liderança de negócio e responsabilidade técnica.
4. **`catalogo_precos.csv` (Dimensão):** Catálogo de preços negociados por produto de dado e fornecedor.

---

## 🛡️ Regras de Governança e Tratamento de Exceções

Uma das principais premissas deste motor é a **resiliência a falhas cadastrais de RH**:

* **Tratamento de Áreas Órfãs (`AREA_99`):** Usuários não mapeados no organograma de áreas têm seus custos direcionados automaticamente para uma conta institucional/contingência (`Área Não Mapeada - Órfão`). Isso impede a quebra do código e permite auditoria posterior sem perda de conciliação.
* **Atribuição Dinâmica de Responsável:** Caso a área não possua um *Líder de Negócio* formal cadastrado (`possui_lider_negocio == 'Nao'`), o sistema realiza o *fallback* para o *Responsável Técnico* (`respo_area`), garantindo a rastreabilidade da aprovação da fatura.
* **Garantia de Conciliação:** A soma de todos os custos rateados é matematicamente igual a 100% da fatura dos fornecedores externos.

---

## 📈 Visões Analíticas e Dashboards Disponíveis

O pipeline processa os dados e entrega 4 visões estratégicas no notebook e na exportação:

1. **Chargeback por Área e Liderança:** Visão de rateio financeiro agrupado por área consumidora e responsável direto.
2. **Share of Wallet por Fornecedor:** Distribuição percentual dos gastos entre os parceiros de dados (Serasa Experian, Banco Central, ClearSale, Serpro).
3. **Ranking de Custos por Produto de Dados:** Mapeamento das tabelas/APIs mais custosas do ecossistema.
4. **Heavy Users & Detecção de Anomalias:** Identificação dos usuários com maior volume financeiro de consultas para prevenção de loops operacionais e consumo desmedido.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3.10+
* **Manipulação de Dados:** Pandas
* **Visualização de Dados:** Matplotlib, Seaborn
* **Formatos de Saída:** CSV (`base_tratada_finops.csv`), PDF (`dashboard_executivo_finops.pdf`), HTML Executivo

---

## 📂 Estrutura do Repositório

```text
.
├── arquivos/
│   ├── consumo_dados.csv          # Base fato de logs
│   ├── usuarios_areas.csv         # Base dimensão usuários
│   ├── areas_lideranca.csv        # Base dimensão áreas
│   ├── catalogo_precos.csv        # Base dimensão catálogo
│   └── base_tratada_finops.csv    # Exportação final consolidada
├── main.ipynb                     # Jupyter Notebook com a pipeline e visualizações
└── README.md                      # Documentação do projeto
```

---

## 🚀 Como Executar

1. **Clonar o repositório:**
   ```bash
   git clone https://github.com/seu-usuario/finops-data-cost-allocation.git
   cd finops-data-cost-allocation
   ```

2. **Instalar as dependências:**
   ```bash
   pip install pandas matplotlib seaborn
   ```

3. **Executar o Jupyter Notebook:**
   ```bash
   jupyter notebook main.ipynb
   ```

---

## 💡 Impacto para o Negócio

* **Prevenção de Leaks Financeiros:** 100% dos custos de APIs externas alocados corretamente aos respectivos centros de custos.
* **Visibilidade Executiva:** Agilidade na renegociação de contratos com fornecedores com base no *Share of Wallet*.
* **Autonomia para Finanças e Negócios:** Dados padronizados para exportação direta em ferramentas de BI (Power BI, Tableau, QuickSight).

---
*Desenvolvido por **Cinthia Marcelino** — Senior Data Analyst | Data Products | Data FinOps*