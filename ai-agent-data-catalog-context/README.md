# 🧠 Ecossistema Multi-Agentes de IA: Autosserviço de Dados e Catálogos

## 🎯 Sobre o Projeto
Este repositório contém a implementação de um **Ecossistema Multi-Agentes de Inteligência Artificial** focado no autosserviço de dados corporativos. A arquitetura foi desenhada para atuar como um portal centralizado de suporte, respondendo a dúvidas de negócios, técnicas e jurídicas sobre o portfólio de dados da empresa.

Em vez de depender de um único modelo de linguagem genérico (sujeito a alucinações financeiras e contratuais), o ecossistema utiliza um padrão de **Orquestração Master/Especialistas** com roteamento baseado em *skills* (ferramentas) e *guardrails* estritos de governança.

---

## 🏗️ Arquitetura de Orquestração (Master/Specialist Pattern)

O sistema simula uma operação de suporte cognitivo em três camadas:

1. **Agente Orquestrador (Master):** Atua como o cérebro central. Recebe a intenção do usuário em linguagem natural, classifica o domínio da pergunta e delega a execução para o Agente Especialista correto, garantindo respostas assertivas.
2. **Especialista em Metadados (Skill 1):** Conectado a uma base estruturada (`CSV`). Especializado em varrer o portfólio da empresa para responder sobre tabelas disponíveis, descrições técnicas de produtos, fornecedores mapeados e status de contratação.
3. **Especialista em Contratos via RAG (Skill 2):** Conectado a uma base de dados não estruturados (`PDF`). Utiliza RAG (*Retrieval-Augmented Generation*) e vetorização para ler instrumentos jurídicos reais e responder exclusivamente sobre vigência, limites de chamadas e custos unitários de APIs.

---

## 🛡️ Governança e Guardrails
A confiabilidade do sistema é garantida por *system prompts* de governança:
* **Anti-Alucinação Financeira:** O Especialista em Contratos é estritamente proibido de calcular volumes totais de consumo ou inventar preços, sendo obrigado a responder "A informação não está disponível" caso o dado não conste no instrumento jurídico vetorizado. A responsabilidade volumétrica total pertence ao Motor de FinOps.
* **Isolamento de Contexto:** Cada especialista enxerga apenas a sua própria base de conhecimento (FAISS Vector Store), garantindo que regras contratuais não se misturem com descrições técnicas de metadados.

---

## 🛠️ Stack Tecnológico
* **Linguagem:** Python (Jupyter Notebook)
* **Framework de Orquestração LLM:** LangChain
* **Modelos:** OpenAI (`gpt-3.5-turbo`, `OpenAIEmbeddings`)
* **Bancos de Dados Vetoriais:** FAISS (*Facebook AI Similarity Search*)
* **Processamento de Dados:** `PyPDFDirectoryLoader` para parsing de contratos e `CSVLoader` para catálogos estruturados.

---

## 📂 Estrutura do Repositório

```text
ai-agent-data-catalog-context/
│
├── contratos_pdf/                  # Base Não Estruturada (PDFs de Contratos/Fornecedores)
│   ├── contrato_serasa.pdf
│   ├── termo_bacen_scr.pdf
│   └── contrato_clearsale.pdf
│
├── portfolio_metadados.csv         # Base Estruturada (Catálogo Interno)
├── main.ipynb                      # Notebook principal com a orquestração do Agente Master e Skills
└── README.md                       # Documentação do projeto
```

---

## 🚀 Como Executar

**1. Instale as dependências:**
Certifique-se de ter o ambiente Jupyter configurado e instale as bibliotecas necessárias:
```bash
pip install langchain langchain-openai pypdf faiss-cpu tiktoken
```

**2. Configuração de API Key:**
O notebook solicitará a inserção segura da sua `OPENAI_API_KEY` utilizando a biblioteca `getpass` na primeira célula, evitando vazamentos no código-fonte.

**3. Execute o Notebook:**
Abra o arquivo `main.ipynb` e execute as células sequencialmente. O console do Jupyter exibirá os logs de "pensamento" (verbose) do Agente Master tomando as decisões de roteamento com base nas perguntas enviadas.