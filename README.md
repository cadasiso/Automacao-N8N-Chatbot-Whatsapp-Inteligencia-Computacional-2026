# 🤖 Automação de Atendimento via WhatsApp - Bike Shop

> **Nota de Contexto:** Este repositório contém os artefatos desenvolvidos como projeto final da disciplina de **Inteligência Computacional** (1º semestre de 2026) no **IFB - Campus Taguatinga**.

O projeto consiste em um protótipo funcional de uma rotina de automação de atendimento para uma empresa de locação, venda e manutenção de bicicletas. O foco principal foi criar um assistente virtual inteligente e integrado aos canais de comunicação reais da empresa, no caso o WhatsApp.

---

## 🛠️ Tecnologias e Arquitetura

O ecossistema do projeto foi desenhado integrando ferramentas que priorizam a flexibilidade e a autonomia do fluxo:

| Ferramenta | Função no Projeto | Justificativa de Escolha |
| :--- | :--- | :--- |
| **n8n** | Orquestrador de Workflow | Permite a construção visual e a fácil manipulação dos nós de IA. |
| **Z-API** | Integração com WhatsApp | Canal principal de contato, garantindo o envio e recebimento de mensagens em tempo real. |
| **OpenRouter** | Roteamento de LLMs | Abstrai provedores (Anthropic, Google, OpenAI) e traz flexibilidade para usar modelos recentes, como o `DeepSeek-v3.2`. |

---

## 🧠 Decisões de Engenharia & IA

### 1. Persistência de Dados Simplificada
Segundo entrevistas com o empresário parceiro, o fluxo de atendimentos possui baixa complexidade e a coleta de dados sensíveis para a retirada de bicicletas ocorre **estritamente de forma presencial**. Por conta disso, a arquitetura de dados foi simplificada:
* Utilização de bancos de dados temporários nativos do n8n (`Data Table`).
* Armazenamento focado na contextualização da IA (histórico de conversas) e consultas rápidas de estoque, preços e políticas internas.

### 2. Controle de Escopo por Master Prompt
O comportamento da IA é regulado por um *Master Prompt* em formato livre (texto corrido). Suas diretrizes obrigatórias são:
* Limitar a atuação estritamente ao universo de bicicletarias.
* Identificar o contexto de horários comerciais e de almoço para o cliente.
* Acionar o gatilho de transição sempre que necessário.

### 3. Transição Automatizada para Atendimento Humano
Caso o cliente demonstre insatisfação, solicite negociações especiais ou faça perguntas fora do escopo, o sistema interrompe o fluxo automatizado e responde com a tag padrão `[TRANSFERIR_ATENDIMENTO]`, preparando o canal para a intervenção de um operador real.

---

## 🚀 Como Replicar este Projeto

### Pré-requisitos
* Uma instância do **n8n** ativa.
* Conta configurada na **Z-API** com uma instância conectada ao WhatsApp.
* Chave de API do **OpenRouter**.

### Passo a Passo
1. Faça o download do arquivo `Cópia do Projeto.json` deste repositório.
2. No seu painel do n8n, crie um novo workflow vazio.
3. No canto superior direito, clique em **Import from File** e selecione o arquivo JSON.
4. Configure as credenciais nos nós da *Z-API*, *OpenRouter* e *Bike Shop Database*.
5. Ative o workflow.

---

## 🔮 Limitações e Melhorias Futuras

Como este projeto trata-se de um protótipo acadêmico (MVP), existem pontos claros de evolução:

* **Centralização de Transbordo:** Inicialmente, cogitou-se a integração com a ferramenta *Chatwoot* para criar uma interface dedicada aos atendentes humanos gerenciarem as filas de chamados. Contudo, devido às restrições de planos gratuitos e custos de implantação para o escopo inicial, a ideia foi postergada. 
* **Persistência de Longo Prazo:** Migrar o banco de dados temporário do n8n para um banco de dados relacional externo (como PostgreSQL), permitindo relatórios e análises futuras de atendimento.
