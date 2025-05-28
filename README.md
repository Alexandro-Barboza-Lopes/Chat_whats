# 📌 Automação Inteligente com n8n, IA e APIs

## 💡 Visão Geral

Desenvolvi uma solução automatizada usando **n8n**, **IA** e **integração de APIs**, focada em interações via **WhatsApp**, com **respostas contextuais** e **memória de conversas**.

---

## 🤖 Projeto: Chatbot WhatsApp com IA e Memória

### 🛠️ Tecnologias Utilizadas

* **n8n** (automação de fluxo)
* **WhatsApp (WAHA)** - API HTTP
* **Google Gemini** (`models/gemini-2.0-flash`)
* **Redis** (memória de conversas)
* **JSON, Webhooks, REST APIs**

### 🔁 Estrutura do Fluxo (n8n)

1. **Webhook:** Recebe mensagens do WhatsApp.
2. **Set (Dados):** Extrai informações como `chatID`, `message`, etc.
3. **Switch:** Valida se o evento é uma mensagem.
4. **AI Agent:** Processa a mensagem com IA e memória via Redis.
5. **Google Gemini:** Geração de resposta com temperatura 0.4.
6. **Redis Chat Memory:** Armazena histórico contextual por 1h.
7. **WAHA (Seen):** Marca mensagens como lidas.
8. **WAHA (Text):** Envia a resposta gerada de volta.

---

## 🧠 Habilidades Demonstradas

* **Automação e Orquestração de Workflows**
* **Integração de APIs (Webhook, WAHA, Google Gemini)**
* **IA e PLN com LLMs**
* **Desenvolvimento de Chatbots Inteligentes**
* **Gerenciamento de Dados com JSON**
* **Memória de Conversa com Redis**
* **Design com Estado e Personalização**
* **Soluções Orientadas a Problemas**

---

## 🔄 Melhorias Futuras

* Integração com CRMs e bases de conhecimento.
* Melhoria no tratamento de erros e logs.
* Suporte a novos tipos de mensagens (áudio, imagem, etc).
* Análise de sentimento e escalonamento para atendimento humano.
* Testes A/B para ajustes de IA e prompts.

---

Este projeto exemplifica minha capacidade de unir **automação e inteligência artificial** para criar **soluções eficazes e escaláveis** em comunicação digital.
