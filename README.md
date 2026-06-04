# 🤖 Edu - Educador Financeiro Virtual com IA Generativa Local

## Contexto

Os assistentes virtuais no setor financeiro estão evoluindo de simples chatbots reativos para **agentes inteligentes e proativos**. O **Edu** é um protótipo funcional de agente inteligente focado em educação financeira que utiliza IA Generativa local para:

- **Antecipar necessidades** interpretando os padrões de consumo antes do usuário pedir.
- **Personalizar** as explicações com base no perfil de risco e objetivos reais de cada cliente.
- **Cocriar soluções** de forma consultiva, ensinando o funcionamento de produtos financeiros.
- **Garantir segurança** absoluta através de diretrizes anti-alucinação rígidas e processamento local.

> [!TIP]
> Esta solução foi desenvolvida utilizando **Ollama** para inferência de LLM local, garantindo que os dados financeiros do cliente fiquem protegidos na máquina e nunca sejam compartilhados externamente.

---

## Solução Entregue: Etapa por Etapa

### 1. Documentação do Agente

Definição de escopo e diretrizes de comportamento do assistente:

- **Caso de Uso:** O Edu resolve a barreira de aprendizado para investidores e poupadores iniciantes, traduzindo conceitos técnicos em linguagem acessível e usando dados práticos da realidade do usuário.
- **Persona e Tom de Voz:** Amigável, empático, extremamente didático e acolhedor (como um amigo com conhecimento avançado em finanças).
- **Segurança (Anti-Alucinação):** Regras explícitas que impedem o agente de fazer recomendações diretas de compra de ativos ou fornecer informações fora de seu escopo técnico.

📄 **Documento:** [`docs/01-documentacao-agente.md`](./docs/01-documentacao-agente.md)

---

### 2. Base de Conhecimento e Análise de Dados

O contexto dinâmico do agente é enriquecido através da leitura em tempo real de bases locais integradas. 

| Arquivo | Formato | Descrição do Uso no Edu |
|---------|---------|-------------------------|
| `transacoes.csv` | CSV | Análise automatizada de despesas e receitas para exemplos práticos. |
| `historico_atendimento.csv` | CSV | Resgata o histórico recente para manter a contextualização e continuidade do chat. |
| `perfil_investidor.json` | JSON | Captura nome, idade, patrimônio total, reserva atual e perfil (ex: Moderado). |
| `produtos_financeiros.json` | JSON | Portfólio de produtos disponíveis (Tesouro Selic, CDB, LCI/LCA, FIIs). |

#### 📊 Processamento Prático das Transações
Como parte do entendimento dos dados, o ecossistema do agente consolida os gastos reais do arquivo `transacoes.csv`. Abaixo está a distribuição gráfica de despesas do cliente usada no contexto de injeção da LLM:

![Resumo de Despesas](./resumo_despesas_edu.png)

📄 **Documento:** [`docs/02-base-conhecimento.md`](./docs/02-base-conhecimento.md)

---

### 3. Prompts do Agente

Modelagem detalhada da Engenharia de Prompt para o direcionamento do modelo:

- **System Prompt:** Definição do papel ("Edu, educador financeiro"), objetivo principal e limitação estrita de respostas rápidas e diretas de até 3 parágrafos.
- **Few-Shot Prompting:** Inclusão de cenários ideais pré-mapeados (perguntas conceituais, consultas de despesas pessoais e dúvidas de investimentos).
- **Tratamento de Edge Cases:** Instruções severas bloqueando respostas sobre previsão do tempo, recusa de compartilhamento de dados de terceiros e tratamento para solicitações de recomendações diretas.

📄 **Documento:** [`docs/03-prompts.md`](./docs/03-prompts.md)

---

### 4. Aplicação Funcional

O protótipo funcional foi totalmente codificado no arquivo principal do sistema, dividindo-se em:

- **Interface Gráfica (Streamlit):** Painel interativo de chat responsivo construído com `st.chat_input`, `st.chat_message` e spinners de carregamento assíncronos.
- **Camada de Dados (Pandas & JSON):** Carregamento estruturado das bases usando chaves seguras (.get()) para mitigar quebras em tempo de execução.
- **Orquestração da LLM (Requests + Ollama):** Criação da função de mensageria que formata dinamicamente o prompt contendo `SYSTEM_PROMPT` + `CONTEXTO_CLIENTE` + `PERGUNTA_USUARIO`, enviando via requisição POST para a API na porta `11434`.

📁 **Código-Fonte:** [`src/app.py`](./src/app.py)

---

### 5. Avaliação e Métricas

Matriz de validação empregada nos testes de aderência do assistente:

- **Cenários Testados:** Consultas sobre alimentação, requisições de indicação direta de investimentos, desvios de escopo comuns e questionamentos sobre papéis de bolsa inexistentes.
- **Formulário de Feedback:** Métricas baseadas em escala de 1 a 5 para **Assertividade** (se respondeu à pergunta), **Segurança** (se respeitou as travas) e **Coerência** (clareza na linguagem).

📄 **Documento:** [`docs/04-metricas.md`](./docs/04-metricas.md)

---

### 6. Pitch

Estruturação da apresentação comercial e técnica do projeto em um formato "Pitch de Elevador" de 3 minutos:

- **Problema (30 seg):** A exclusão financeira e o medo de errar de 62% dos brasileiros por falta de conhecimento didático.
- **Solução (60 seg):** Apresentação do Edu, o professor particular de finanças focado no contexto do próprio usuário.
- **Demo (60 seg):** Demonstração prática respondendo "O que é CDI?" e mapeando as maiores despesas em tempo real.
- **Diferencial (30 seg):** Solução open-source, rodando 100% local, custo zero de tokens e garantia absoluta de privacidade de dados sensíveis.

📄 **Documento:** [`docs/05-pitch.md`](./docs/05-pitch.md)

---

## Ferramentas e Bibliotecas Utilizadas

| Categoria | Tecnologia | Uso no Projeto |
|-----------|------------|----------------|
| **LLM Engine** | [Ollama](https://ollama.ai/) | Motor de inferência local rodando o modelo `gpt-oss`. |
| **Interface** | [Streamlit](https://streamlit.io/) | Frontend completo para a aplicação interativa de chat. |
| **Data Engine** | [Pandas](https://pandas.pydata.org/) | Manipulação de arquivos tabulares (.csv) e filtragem de transações. |
| **Comunicação**| [Requests](https://requests.readthedocs.io/) | Integração de backend enviando as requisições para a API local do modelo. |

📁 edu-agente-financeiro/
│
├── 📄 README.md                      # Documentação principal da solução
├── 📄 resumo_despesas_edu.png        # Gráfico estatístico extraído do motor de dados
│
├── 📁 data/                          # Base de conhecimento integrada do cliente
│   ├── historico_atendimento.csv     # Extrato das últimas interações
│   ├── perfil_investidor.json        # Dados de perfil, objetivos e patrimônio
│   ├── produtos_financeiros.json     # Catálogo conceitual de investimentos
│   └── transacoes.csv                # Extrato de receitas e saídas do cliente
│
├── 📁 docs/                          # Engenharia de prompts e documentações
│   ├── 01-documentacao-agente.md     # Escopo de caso de uso e persona
│   ├── 02-base-conhecimento.md       # Estratégia e adaptações de dados
│   ├── 03-prompts.md                 # System prompts e travas anti-alucinação
│   ├── 04-metricas.md                # Cenários de teste e formulário de avaliação
│   └── 05-pitch.md                   # Roteiro cronometrado da solução
│
└── 📁 src/                           # Backend e aplicação do sistema
└── app.py                        # Script integrado em Python (Streamlit + LLM Client)

---

