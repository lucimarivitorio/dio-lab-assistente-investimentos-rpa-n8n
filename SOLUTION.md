# 🚀 Assistente de Investimentos com RPA e IA Generativa

Projeto desenvolvido como desafio prático pela **DIO (Digital Innovation One)**, combinando **RPA (Robotic Process Automation)** em Python, **Workflows de Automação** no n8n e **IA Generativa (Google Gemini)** para personalização de comunicação no mercado financeiro.

---

## 👤 Autora
- **Nome:** Lucimari Vitório 🌟
- **Projeto:** Assistente de Investimentos com RPA e IA Generativa

---

## 📌 Visão Geral do Projeto

O objetivo principal deste pipeline é automatizar a coleta de dados de clientes na web, cruzar essas informações com opções de investimentos baseadas no perfil financeiro de cada um e gerar recomendações de e-mail personalizadas e humanizadas via LLM.

<img width="1052" height="388" alt="image" src="https://github.com/user-attachments/assets/c30916fe-3e31-4d28-b314-c89e49f2a018" />

---

## ⚙️ Detalhamento das Etapas e Decisões Técnicas

1. **Extração de Dados (RPA em Python):**
   - O notebook `rpa/extrair_clientes.ipynb` utiliza a biblioteca `BeautifulSoup` para extrair a tabela de clientes da página simulada (`docs/index.html`).
   - Os dados coletados (Nome, E-mail, Saldo, Perfil) são enviados via requisição HTTP POST para a URL do Webhook no n8n.

2. **Consumo de Dados de Investimento:**
   - O nó `HTTP Request` no n8n faz o download da base oficial de opções de investimento armazenada em `docs/data.csv`.

3. **Cruzamento de Perfis & Formatação:**
   - Através de nós `Code` em JavaScript e `Merge`, os dados de cada cliente são combinados com os produtos adequados aos seus perfis (*Conservador*, *Moderado* e *Arrojado*).

4. **Geração Dinâmica de E-mails com IA Generativa:**
   - O nó do **Google Gemini** (`models/gemini-3.5-flash-lite`) recebe os dados do cliente e gera a mensagem em formato JSON estruturado com os campos `subject`, `text_body` e `html_body`.
   - **Decisão Técnica:** O prompt foi configurado no modo `Fixed` do n8n com expressões `{{ $json.nome }}`, garantindo a substituição dos dados dinâmicos do cliente antes da chamada da API do Gemini.

5. **Tratamento e Padronização no n8n:**
   - O nó `Code 4` realiza o parse do JSON devolvido pelo Gemini e preserva a correspondência exata dos e-mails de destino (`email_to`) para cada um dos clientes por índice.

6. **Envio e Validação Condicional:**
   - Utilização do nó `If` para direcionar a execução e envio seguro via nó do **Gmail** autenticado com **OAuth2**, finalizando com a resposta do Webhook.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

- **Linguagem / Scraping:** Python (`BeautifulSoup`, `requests`, `pandas`).
- **Orquestração de Processos:** n8n Cloud.
- **Modelo de IA Generativa:** Google Gemini API (`models/gemini-3.5-flash-lite`).
- **Autenticação de E-mail:** Gmail API com credenciais OAuth2.
- **Hospedagem de Dados:** GitHub Pages (`index.html` e `data.csv`).

---

## 📂 Estrutura Final do Repositório

```text
├── docs/
│   ├── index.html               # Página web simulada com a tabela de clientes
│   └── data.csv                 # Base com as opções de investimento por perfil
├── rpa/
│   └── extrair_clientes.ipynb   # Notebook Python RPA integrado ao Webhook
├── workflowjson/
│   └── workflow.json            # Workflow exportado do n8n
├── SOLUTION.md                  # Documentação completa da solução
└── README.md                    # Instruções originais do repositório
