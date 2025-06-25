# Bot de Suporte Técnico de Primeiro Nível (FAQ) com n8n e IA

---

## 🚀 Visão Geral do Projeto

Este projeto demonstra a construção de um bot de suporte técnico automatizado para o Telegram. Ele atua como um sistema de FAQ de primeiro nível, utilizando uma base de conhecimento no Google Sheets e a inteligência artificial do **Deepseek (acessada via OpenRouter)** para responder às dúvidas dos usuários. Se o bot não conseguir resolver o problema com sua base de conhecimento e lógica de troubleshooting, ele orienta o usuário a aguardar o contato de um atendente humano.

O foco principal é a automação com **low-code (n8n)** e a integração de **Inteligência Artificial (Deepseek via OpenRouter)** para um atendimento eficiente e escalável, superando desafios de acesso à API.

## ✨ Tecnologias Utilizadas

* **n8n.io:** Plataforma de automação low-code para orquestrar o fluxo do bot. Utilizado na versão Cloud (com free tier).
* **Google Sheets:** Base de dados para armazenar perguntas frequentes e suas respectivas respostas.
* **Telegram BotFather:** Para criação e gerenciamento da interface do bot no Telegram (`CondeTechHelper-Bot`).
* **Deepseek API (via OpenRouter):** Modelo de Inteligência Artificial (`deepseek/deepseek-chat-v3-0324:free`) para processamento de linguagem natural (PLN) e busca/formulação de respostas relevantes na FAQ, incluindo troubleshooting. Acesso facilitado pelo OpenRouter, que oferece este modelo gratuitamente e sem consumo de créditos.
* **JavaScript (no nó 'Code' do n8n):** Utilizado para pré-processamento e agregação da base de conhecimento antes de enviá-la à IA.

## ⚙️ Arquitetura do Sistema

[**DIAGRAMA SIMPLES DO FLUXO:** Usuário -> Telegram -> n8n (Telegram Trigger -> Get row(s) in sheet -> **Code (Agregação FAQ)** -> AI Agent -> Telegram Send Message). Pode usar ferramentas como draw.io e adicionar a imagem na pasta `screenshots/`]

## 🚀 Como Funciona

1.  O usuário envia uma mensagem com sua dúvida para o bot no Telegram.
2.  O `Telegram Trigger` do n8n recebe essa mensagem via Webhook.
3.  O nó `Get row(s) in sheet` acessa a base de conhecimento completa (FAQ) do Google Sheets.
4.  Um nó `Code` então agrega todas as perguntas e respostas da FAQ em uma única string formatada.
5.  O nó `AI Agent` (com o modelo **Deepseek via OpenRouter**) recebe:
    * A pergunta do usuário (da mensagem do Telegram).
    * A base de conhecimento agregada (do nó `Code`) como parte de seu prompt de sistema altamente restritivo.
    * A IA processa a pergunta, consulta a base de conhecimento e usa sua inteligência para formular uma resposta prestativa, que pode incluir passos de troubleshooting.
6.  O n8n envia a resposta gerada pela IA de volta para o usuário no Telegram através do nó `Telegram (Send Message)`.
7.  Em caso de não resolução ou pergunta complexa, o bot informa que um atendente entrará em contato, conforme instrução no prompt da IA.

## 🛠️ Configuração (Passo a Passo)

### 1. Google Sheets (Base de Conhecimento)

* **Criada** uma planilha com as colunas `Pergunta` e `Resposta`.
* **Preenchemos** com (inicialmente) 60 perguntas e respostas sobre problemas básicos de informática, gerando uma FAQ de suporte técnico.
* **Planilha compartilhada** para **leitura** com "Qualquer pessoa com o link".
* **[LINK PARA A PLANILHA AQUI](https://docs.google.com/spreadsheets/d/1dsOyz_VSDe_pBo9hRvWSaxJHNKUAvocDUNoGwk4yuvI/edit?usp=sharing)**
* Você pode ver um exemplo da estrutura em `faq_example.csv` neste repositório.

### 2. Telegram Bot (`CondeTechHelper-Bot` via BotFather)

* **Criamos** um novo bot com o `@BotFather` no Telegram, nomeando-o `CondeTechHelper-Bot`.
* **Obtivemos** o **HTTP API Token** desse Bot, essencial para a comunicação com o n8n.

### 3. n8n.io (Construção e Ativação do Workflow)

* **Criamos** uma conta na versão Cloud do n8n. (Possui período de trial e free tier mensal limitado, mas suficiente para portfólio).
* **Construímos o workflow com os seguintes nós:**
    1.  **`Telegram Trigger (On message)`**: Para receber as mensagens dos usuários.
        * Configurado com a credencial do Telegram (usando o HTTP API Token do `CondeTechHelper-Bot`).
    2.  **`Get row(s) in sheet`**: Para ler a base de conhecimento do Google Sheets.
        * Configurado com a credencial do Google Sheets, o Spreadsheet ID e o Sheet Name.
        * Operação: `Get row(s) in sheet`.
    3.  **`Code`**: Para agregar as 60 linhas da FAQ em uma única string de texto.
        * Script JavaScript para iterar sobre `items` (linhas da planilha) e concatenar `item.json.Pergunta` e `item.json.Resposta` em uma variável `aggregatedFaq`, retornando-a como `return [{ json: { faq_content: aggregatedFaq } }];`.
    4.  **`AI Agent`**: O cérebro do bot, utilizando o **Deepseek**.
        * Configurado com o `OpenRouter Chat Model` (ver próxima seção).
        * `System Prompt` altamente restritivo (para não repetir a base de conhecimento).
        * A "Base de Conhecimento" no prompt aponta para `{{ $node["Code"].json.faq_content }}`.
        * `User Message` aponta para `{{ $node["Telegram Trigger"].json["message"]["text"] }}`.
    5.  **`Telegram (Send Message)`**: Para enviar a resposta da IA de volta para o usuário.
        * Configurado com a mesma credencial do Telegram.
        * `Chat ID` aponta para `{{ $node["Telegram Trigger"].json["message"]["chat"]["id"] }}`.
        * `Text` aponta para `{{ $node["AI Agent"].json["text"] }}`.
* **Ativamos o workflow** clicando no botão "Active" no canto superior direito da interface do n8n, deixando o bot rodando continuamente.

### 4. OpenRouter API Key (para Deepseek)

* **Acessamos** o OpenRouter.ai para **gerar** a API Key que nos permite utilizar o modelo **Deepseek (`deepseek/deepseek-chat-v3-0324:free`)** no nosso projeto. Este modelo é gratuito via OpenRouter e não consome créditos.
* **Configuramos** o nó `OpenRouter Chat Model` com esta API Key e selecionamos o modelo `deepseek/deepseek-chat-v3-0324:free`.

## 🖼️ Screenshots do Workflow e Demonstração

[**AQUI VOCÊ VAI ADICIONAR IMAGENS:** do seu workflow completo no n8n (com todos os nós visíveis), e screenshots/vídeos da interação com o bot no Telegram mostrando ele respondendo corretamente. Guarde essas imagens na pasta `screenshots/` e referencie-as aqui.]

## 💡 Próximos Passos e Melhorias Futuras

* Implementar um sistema de log para registrar as perguntas não respondidas pela IA para futuras melhorias na FAQ.
* Adicionar um botão inline no Telegram para "Falar com Atendente" que dispara uma notificação para uma equipe de suporte (via e-mail, Slack ou outro Telegram).
* Expandir a base de conhecimento com mais categorias de problemas e soluções avançadas.
* Explorar a adição de "memória" ao bot para conversas mais complexas e contextuais.

## 🤝 Contato

Se você tiver alguma dúvida ou sugestão sobre este projeto, sinta-se à vontade para entrar em contato:

* Gabriel Arten Conde
* **LinkedIn:** [https://www.linkedin.com/in/gabriel-arten-conde/](https://www.linkedin.com/in/gabriel-arten-conde/)
* **Email:** [gabriel_x5@hotmail.com](mailto:gabriel_x5@hotmail.com)

---
