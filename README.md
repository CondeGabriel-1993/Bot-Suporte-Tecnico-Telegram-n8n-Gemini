# Bot de Suporte Técnico de Primeiro Nível (FAQ) com n8n e IA

---

## 🚀 Visão Geral do Projeto

Este projeto demonstra a construção de um bot de suporte técnico automatizado para o Telegram. Ele atua como um sistema de FAQ de primeiro nível, utilizando uma base de conhecimento no Google Sheets e a inteligência artificial do Google Gemini para responder às dúvidas dos usuários. Se o bot não conseguir resolver o problema, ele orienta o usuário a aguardar o contato de um atendente humano.

O foco principal é a automação com **low-code (n8n)** e a integração de **Inteligência Artificial (Google Gemini)** para um atendimento eficiente e escalável.

## ✨ Tecnologias Utilizadas

* **n8n.io:** Plataforma de automação low-code para orquestrar o fluxo do bot.
* **Google Sheets:** Base de dados para armazenar perguntas frequentes e suas respectivas respostas.
* **Telegram BotFather:** Para criação e gerenciamento da interface do bot no Telegram.
* **Google Gemini API:** Modelo de Inteligência Artificial para processamento de linguagem natural (PLN) e busca de respostas relevantes na FAQ.

## ⚙️ Arquitetura do Sistema

[**DIAGRAMA SIMPLES DO FLUXO:** Usuário -> Telegram -> n8n -> Google Sheets/Gemini -> n8n -> Telegram. Pode usar ferramentas como draw.io e adicionar a imagem na pasta `screenshots/`]

## 🚀 Como Funciona

1.  O usuário envia uma mensagem com sua dúvida para o bot no Telegram.
2.  O Telegram encaminha a mensagem para o workflow do n8n via Webhook.
3.  O n8n acessa a base de conhecimento (Google Sheets) e a envia para o Agente de IA (Google Gemini) junto com a pergunta do usuário.
4.  O Google Gemini processa a pergunta, compara com a FAQ e retorna a resposta mais relevante ou uma mensagem padrão de "não encontrado".
5.  O n8n envia a resposta final de volta para o usuário no Telegram.
6.  Em caso de não resolução, o bot informa que um atendente entrará em contato.

## 🛠️ Configuração (Passo a Passo)

### 1. Google Sheets (Base de Conhecimento)

* Criada uma planilha com as colunas `Pergunta` e `Resposta`.
* Preenchemos com (inicialmente) 50 perguntas e respostas sobre problemas basicos de informatica, gerando uma FAQ de suporte técnico.
* Planilha compartilhada para **leitura** com "Qualquer pessoa com o link".
* **[[LINK PARA A PLANILHA AQUI](https://docs.google.com/spreadsheets/d/1dsOyz_VSDe_pBo9hRvWSaxJHNKUAvocDUNoGwk4yuvI/edit?usp=sharing)]**
* Você pode ver um exemplo da estrutura em `faq_example.csv` neste repositório.

### 2. Telegram Bot (via BotFather)

* Criamos um novo bot com o `@BotFather` no Telegram.
* Obtemos **HTTP API Token** desse Bot.

### 3. n8n.io (Construção do Workflow)

* Criamos uma conta na versão Cloud do n8n.
* Construimos o workflow seguindo os passos detalhados na seção [AQUI VOCÊ VAI ADICIONAR O LINK PARA O ARQUIVO `workflow.json` E TALVEZ UMA IMAGEM DO SEU WORKFLOW].
* Configuramos os nós para Telegram, Google Sheets e Agente de IA (Google Gemini).

### 4. Google Gemini API Key

* Acessamos o Google AI Studio para gerar a API Key gratuita para utilizar o Gemini no nosso projeto.

## 🖼️ Screenshots do Workflow e Demonstração

[**AQUI VOCÊ VAI ADICIONAR IMAGENS:** do seu workflow no n8n, da interação com o bot no Telegram. Guarde essas imagens na pasta `screenshots/` e referencie-as aqui.]

## 💡 Próximos Passos e Melhorias Futuras

* Implementar um sistema de log para registrar as perguntas não respondidas pela IA.
* Adicionar um botão no Telegram para "Falar com Atendente" que dispara uma notificação via e-mail/Slack.
* Expandir a base de conhecimento com mais categorias de problemas.

## 🤝 Contato

Se você tiver alguma dúvida ou sugestão sobre este projeto, sinta-se à vontade para entrar em contato:

* **Seu Nome:** [Gabriel Arten Conde]
* **LinkedIn:** [https://www.linkedin.com/in/gabriel-arten-conde/]
* **Email:** [gabriel_x5@hotmail.com]

---
