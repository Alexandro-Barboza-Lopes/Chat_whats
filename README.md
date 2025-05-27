## Projeto: Automação Inteligente com n8n, IA e APIs

Este portfólio demonstra minhas habilidades na criação de soluções automatizadas e inteligentes, com foco na integração de diversas tecnologias para construir fluxos de trabalho eficientes e responsivos. O projeto detalhado abaixo exemplifica minha capacidade de desenvolver chatbots avançados e gerenciar interações complexas.

---

### Projeto: Chatbot WhatsApp com Inteligência Artificial e Memória de Conversa

**Visão Geral:**
Este projeto consiste em um fluxo de trabalho automatizado construído na plataforma n8n, projetado para gerenciar interações via WhatsApp. O sistema recebe mensagens, as processa utilizando um modelo de Inteligência Artificial (Google Gemini) para gerar respostas contextuais e mantém um histórico da conversa utilizando Redis para interações mais ricas e personalizadas. As respostas são então enviadas de volta ao usuário através do WhatsApp.

**Tecnologias e Ferramentas Utilizadas:**

* **Automação de Fluxo de Trabalho:** n8n
* **Plataforma de Mensagens:** WhatsApp (integrado via WAHA - WhatsApp HTTP API)
* **Inteligência Artificial:** Google Gemini (modelo `models/gemini-2.0-flash`)
* **Memória de Conversa:** Redis
* **Manipulação de Dados:** JSON
* **Integração de APIs:** Webhooks, APIs REST (WAHA, Google Gemini)

**Detalhes do Fluxo de Trabalho (n8n):**

O fluxo de trabalho é composto pelos seguintes nós principais:

1.  **`Webhook` (Receptor de Mensagens):**
    * **Tipo:** `n8n-nodes-base.webhook`
    * **Função:** Atua como o ponto de entrada, recebendo notificações de novas mensagens do WhatsApp via método POST.

2.  **`Dados` (Extração e Preparação de Dados):**
    * **Tipo:** `n8n-nodes-base.set`
    * **Função:** Extrai e armazena informações cruciais do payload da mensagem recebida, como:
        * `session`: Identificador da sessão do WhatsApp.
        * `chatID`: Identificador único do chat/usuário.
        * `pushName`: Nome de exibição do remetente.
        * `pyload_id`: ID da mensagem original.
        * `event`: Tipo de evento (ex: "message").
        * `message`: Conteúdo da mensagem do usuário.
        * `from_ME`: Booleano indicando se a mensagem foi enviada pelo próprio bot.

3.  **`Switch` (Roteamento Lógico):**
    * **Tipo:** `n8n-nodes-base.switch`
    * **Função:** Direciona o fluxo com base em condições. Neste caso, verifica se o `event` é igual a "message" para prosseguir com o processamento da IA.

4.  **`AI Agent` (Processamento com Inteligência Artificial):**
    * **Tipo:** `@n8n/n8n-nodes-langchain.agent`
    * **Função:** Orquestra a interação com o modelo de linguagem e a memória.
        * **Prompt:** Utiliza o `message` do usuário como entrada para o modelo de IA.
        * **System Message:** "Você é especialista em engenharia de prompt com dez anos de experiência, você ira ajudar o melhor prompt com muita dedicação."
        * **Conexões:**
            * Conectado ao `Google Gemini Chat Model` para geração de texto.
            * Conectado ao `Redis Chat Memory` para persistência do histórico da conversa.

5.  **`Google Gemini Chat Model` (Modelo de Linguagem):**
    * **Tipo:** `@n8n/n8n-nodes-langchain.lmChatGoogleGemini`
    * **Função:** Gera respostas baseadas no prompt fornecido pelo `AI Agent`.
        * **Modelo:** `models/gemini-2.0-flash`
        * **Temperatura:** 0.4 (controla a criatividade/aleatoriedade da resposta).

6.  **`Redis Chat Memory` (Memória de Conversa):**
    * **Tipo:** `@n8n/n8n-nodes-langchain.memoryRedisChat`
    * **Função:** Armazena e recupera o histórico das conversas para fornecer contexto ao `AI Agent`.
        * **Chave de Sessão:** `{{ $('Dados').item.json.chatID }}` (usa o ID do chat para individualizar as conversas).
        * **TTL da Sessão:** 3600 segundos (1 hora).
        * **Tamanho da Janela de Contexto:** 10 mensagens.

7.  **`WAHA` (Marcar Mensagem como Lida):**
    * **Tipo:** `n8n-nodes-waha.WAHA`
    * **Função:** Envia uma confirmação de "mensagem lida" para o WhatsApp.
        * **Operação:** `Send Seen`
        * **Parâmetros:** Utiliza `session`, `chatID` e `pyload_id` do nó `Dados`.

8.  **`WAHA1` (Enviar Resposta):**
    * **Tipo:** `n8n-nodes-waha.WAHA`
    * **Função:** Envia a resposta gerada pelo `AI Agent` de volta para o usuário no WhatsApp.
        * **Operação:** `Send Text`
        * **Texto:** `{{ $('AI Agent').item.json.output }}` (saída do nó `AI Agent`).
        * **Parâmetros:** Utiliza `session` e `chatID` do nó `Dados`.

---

**Habilidades Demonstradas:**

* **Automação de Processos e Orquestração de Workflows:** Proficiência no design e implementação de fluxos de trabalho complexos e automatizados utilizando n8n.
* **Integração de APIs:** Experiência sólida na conexão e utilização de diversas APIs (Webhook para recebimento, WhatsApp HTTP API para envio/recebimento, API do Google Gemini para IA, Redis para banco de dados).
* **Inteligência Artificial e Processamento de Linguagem Natural (PLN):** Capacidade de integrar e utilizar Modelos de Linguagem Grandes (LLMs) como o Google Gemini para compreensão de linguagem natural, geração de respostas e engenharia de prompts.
* **Desenvolvimento de Chatbots:** Habilidade na construção de chatbots interativos, inteligentes e com estado (memória).
* **Gerenciamento de Dados:** Competência na extração, transformação e gerenciamento de dados em formato JSON dentro de um fluxo de trabalho.
* **Design de Aplicações com Estado:** Implementação de memória de conversa utilizando Redis para permitir interações contextuais e personalizadas.
* **Resolução de Problemas:** Capacidade de desenhar sistemas para automatizar respostas e otimizar a comunicação.
* **Conhecimento em Ferramentas Específicas:** n8n, Google Gemini, Redis, APIs do WhatsApp.

**Possíveis Melhorias e Evoluções Futuras:**

* Integração com sistemas de backend (CRMs, bancos de dados de conhecimento) para respostas ainda mais ricas e personalizadas.
* Implementação de tratamento de erros e logging mais robustos.
* Expansão da lógica do `Switch` para lidar com diferentes tipos de mensagens (mídia, comandos específicos).
* Adição de análise de sentimento para adaptar o tom das respostas ou escalar interações para atendimento humano.
* Testes A/B com diferentes prompts, modelos de IA ou configurações para otimizar a performance e a qualidade das respostas.

---

Este projeto demonstra minha paixão por criar soluções inovadoras e eficientes, combinando o poder da automação com a inteligência artificial para resolver desafios reais de comunicação e interação. Estou sempre buscando aprender e aplicar novas tecnologias para entregar resultados de alto impacto.
