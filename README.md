# Projeto de TCC: Fluxos de Trabalho n8n com IA

Este repositório contém os fluxos de trabalho (workflows) n8n desenvolvidos para o meu Trabalho de Conclusão de Curso (TCC). Os fluxos demonstram a integração de um chatbot no Telegram com modelos de linguagem da Google (Gemini) para criar um assistente de vendas virtual para uma loja de roupas.

## 📌 Visão Geral dos Cenários

Existem dois cenários principais neste projeto:

1.  **Cenário 1: Boas-Vindas Simples**
    *   Um bot simples que responde a qualquer mensagem no Telegram com uma saudação de boas-vindas gerada por IA. É uma demonstração básica da conexão entre as plataformas.

2.  **Cenário 2: Assistente de Vendas com Catálogo**
    *   Um bot mais avançado que atua como uma assistente de estilo chamada "Clô".
    *   Ela consegue entender a intenção do usuário (saudação ou interesse em produtos).
    *   Mantém o estado da conversa usando um banco de dados Redis.
    *   Quando o usuário pede para ver os produtos, o fluxo consulta uma API local, busca os itens e os apresenta de forma atraente no chat, com fotos, descrições e preços.
    *   A IA é usada tanto para classificar a intenção do usuário quanto para gerar textos criativos para a apresentação dos produtos.

---

## ⚙️ Configuração e Pré-requisitos

Para executar esses fluxos de trabalho, você precisará da seguinte configuração:

### 1. n8n

*   Uma instância do n8n rodando. Você pode usar o n8n Desktop, n8n Cloud ou auto-hospedar com Docker.
*   **Como importar:** Faça o download dos arquivos `.json` da pasta `fluxos` e importe-os diretamente na sua interface do n8n.

### 2. Credenciais

Você precisará configurar as seguintes credenciais no n8n antes de ativar os fluxos:

*   **Telegram Bot Token:**
    1.  Converse com o [@BotFather](https://t.me/BotFather) no Telegram.
    2.  Crie um novo bot usando o comando `/newbot`.
    3.  Ele fornecerá um token de acesso.
    4.  No n8n, vá em **Credentials > New**, procure por "Telegram" e adicione seu token.

*   **Google Gemini (PaLM) API:**
    1.  Acesse o [Google AI Studio](https://aistudio.google.com/).
    2.  Clique em **"Get API key"** para gerar sua chave.
    3.  No n8n, vá em **Credentials > New**, procure por "Google Gemini (PaLM) API" e adicione sua chave.

*   **Redis:**
    1.  Este projeto foi configurado para usar um banco de dados Redis local.
    2.  No n8n, vá em **Credentials > New**, procure por "Redis" e configure o acesso ao seu servidor (normalmente, se estiver rodando localmente, o host será `localhost` e a porta `6379`).

### 3. Dependências Externas (Apenas para o Cenário 2)

O **Cenário 2** depende de dois serviços externos que precisam estar em execução:

*   **Banco de Dados Redis:**
    *   Necessário para salvar o estado da conversa com cada usuário.
    *   Você pode rodar o Redis facilmente usando Docker:
        ```bash
        docker run --name meu-redis -p 6379:6379 -d redis
        ```

*   **API de Produtos Local:**
    *   O fluxo de trabalho faz uma requisição `GET` para `http://host.docker.internal:3000/produtos` para buscar o catálogo de produtos.
    *   `host.docker.internal` é um DNS especial que permite que o container do n8n (se você estiver usando Docker) acesse um serviço rodando na sua máquina local (host).
    *   Você precisa ter uma API rodando na porta 3000 da sua máquina que retorne um JSON com a lista de produtos. Cada produto no JSON deve ter, no mínimo, os campos: `nome`, `descricao`, `preco` e `imagem_url`.

---

## ▶️ Como Executar

1.  **Configure o Ambiente:** Garanta que o Redis e a API de produtos (para o Cenário 2) estejam rodando.
2.  **Importe os Fluxos:** Importe os arquivos `cenario_1.json` e `cenario_2.json` para o seu n8n.
3.  **Associe as Credenciais:** Abra cada fluxo e associe os nós que pedem credenciais (Telegram, Google Gemini, Redis) com as credenciais que você criou.
4.  **Ative os Fluxos:** Ative o fluxo que deseja testar clicando no botão "Active" no canto superior direito.
5.  **Teste no Telegram:** Envie uma mensagem para o seu bot no Telegram e veja a mágica acontecer!