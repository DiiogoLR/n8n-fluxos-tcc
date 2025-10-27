# Projeto TCC: Automação de E-commerce com n8n e IA Generativa

Este repositório contém os fluxos de trabalho (workflows) desenvolvidos em [n8n](https://n8n.io/) para o meu Trabalho de Conclusão de Curso. O projeto demonstra a criação de um assistente de vendas virtual para uma loja de roupas, integrando um chatbot no Telegram com a IA Generativa da Google (Gemini).

![n8n-telegram-gemini-redis](https-github-com-DiiogoLR-n8n-fluxos-tcc/assets/75768399/16a11756-ef2c-473d-8ab3-7711f19c3b17)

## 📌 Visão Geral dos Cenários

O projeto é dividido em três fluxos de trabalho principais, cada um representando um nível de complexidade e funcionalidade.

| Cenário | Nome do Fluxo | Descrição |
| :--- | :--- | :--- |
| **1** | `Boas-Vindas Simples` | Um bot de entrada que responde a qualquer mensagem com uma saudação calorosa gerada por IA. Ideal para validar a conexão entre Telegram e Gemini. |
| **2** | `Assistente de Vendas` | Uma assistente virtual (Clô) que gerencia uma conversa completa, entende a intenção do usuário, consulta um catálogo de produtos via API, e salva o estado da conversa em um banco de dados Redis. |
| **3** | `Notificação de Lançamento`| Um sistema proativo que, ao receber um novo produto via webhook, busca no Redis todos os usuários interessados naquela categoria e envia uma notificação de lançamento personalizada com IA. |

---

## ✨ Tecnologias Utilizadas

* **Plataforma de Automação:** [n8n](https://n8n.io/)
* **Inteligência Artificial:** [Google Gemini (via Google AI Studio)](https://aistudio.google.com/)
* **Mensageria:** [Telegram](https://telegram.org/)
* **Banco de Dados:** [Redis](https://redis.io/) (Para gerenciamento de estado da conversa e inscrições)
* **API Local:** [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/) (Para simular o catálogo de produtos)
* **Orquestração:** [Docker](https://www.docker.com/)

---

## ⚙️ Configuração e Pré-requisitos

Para executar esses fluxos de trabalho, você precisará da seguinte configuração:

### 1. n8n
* Uma instância do n8n rodando. Você pode usar o **n8n Desktop**, **n8n Cloud** ou auto-hospedar com **Docker**.
* **Como importar:** Faça o download dos arquivos `.json` da pasta `fluxos` e importe-os diretamente na sua interface do n8n.

### 2. Credenciais
Você precisará configurar as seguintes credenciais no n8n (`Credentials > Add credential`):

* **Telegram Bot Token:**
    1.  Converse com o [@BotFather](https://t.me/BotFather) no Telegram e use o comando `/newbot`.
    2.  Ele fornecerá um token de acesso.
    3.  No n8n, adicione uma credencial do tipo "Telegram Bot API".

* **Google Gemini API:**
    1.  Acesse o [Google AI Studio](https://aistudio.google.com/).
    2.  Clique em **"Get API key"** para gerar sua chave.
    3.  No n8n, adicione uma credencial do tipo "Google Gemini API".

* **Redis:**
    1.  No n8n, adicione uma credencial do tipo "Redis".
    2.  Se estiver rodando o Redis localmente via Docker, o host será `localhost` e a porta `6379`.

### 3. Dependências Externas (Para Cenários 2 e 3)
Os cenários mais avançados dependem de dois serviços rodando em sua máquina:

* **Banco de Dados Redis:**
    * Necessário para salvar o estado da conversa e as inscrições de categoria.
    * A maneira mais fácil de subir uma instância é com Docker:
      ```bash
      docker run --name meu-redis -p 6379:6379 -d redis
      ```

* **API de Produtos Local:**
    * A API é uma aplicação Node.js simples que serve os dados dos produtos.
    * O fluxo do n8n está configurado para acessar a API em `http://host.docker.internal:3000`. Este DNS especial permite que o container do n8n (se estiver usando Docker) acesse um serviço rodando na sua máquina local (host).
    * Para rodar a API, siga os passos abaixo.

---

### Como Rodar a API Local

1.  **Navegue até a pasta da API:**
    ```bash
    cd api
    ```

2.  **Instale as dependências:**
    ```bash
    npm install
    ```

3.  **Inicie a API:**
    ```bash
    node app.js
    ```
    A API estará rodando em `http://localhost:3000`.

---

## ▶️ Como Executar os Fluxos

1.  **Prepare o Ambiente:** Garanta que o **Redis** e a **API de produtos** (para os Cenários 2 e 3) estejam rodando.
2.  **Importe os Fluxos:** Importe os arquivos `.json` desejados para a sua instância do n8n.
3.  **Associe as Credenciais:** Em cada fluxo, abra os nós que exigem autenticação (Telegram, Google Gemini, Redis) e selecione as credenciais que você acabou de criar.
4.  **Ative os Fluxos:** Ative o fluxo que deseja testar clicando no botão **"Active"** no canto superior direito.
5.  **Teste no Telegram:** Envie uma mensagem para o seu bot e veja a mágica acontecer!

---

## 📁 Estrutura do Repositório
. ├── api/ # Contém a API local em Node.js │ ├── app.js # Arquivo principal da API │ ├── db.json # Simulação de banco de dados com produtos e categorias │ └── package.json │ ├── fluxos/ # Arquivos de exportação dos workflows do n8n │ ├── Cenario_1.json │ ├── Cenario_2.json │ └── Cenario_3.json │ └── README.md