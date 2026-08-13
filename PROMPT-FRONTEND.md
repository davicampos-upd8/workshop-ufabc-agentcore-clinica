# Prompt — Integração do front-end

Quero conectar o front-end pré-construído disponível em `frontend/index.html` à API do agente da clínica Nova Essência.

O agente e a API com AWS Lambda e Amazon API Gateway já foram construídos. Nesta etapa, não recrie a Knowledge Base, o AgentCore Harness nem o backend.

## Integração esperada

- Configurar no front-end a URL do API Gateway criado anteriormente.
- Enviar as mensagens para `POST /chat` no formato:
  ```json
  {
    "message": "texto digitado pela cliente",
    "sessionId": "identificador persistente da conversa"
  }
  ```
- Exibir no chat o campo `reply` retornado pela API.
- Manter um `sessionId` por navegador para preservar a memória da conversa.
- Mostrar um estado visual enquanto a resposta está sendo processada.
- Tratar falhas com uma mensagem amigável, sem mostrar stack traces, ARNs ou detalhes internos da AWS.
- Manter as sugestões de mensagens já existentes e preservar o visual atual da página.
- Garantir que a interface continue funcionando bem em desktop e celular.

## Publicação

Publicar o front-end como site estático no Amazon S3, usando o prefixo definido em `WORKSHOP_PREFIX`.

Ajustar o CORS do API Gateway para aceitar requisições do endereço publicado. Nenhuma credencial AWS pode ser incluída no HTML ou no JavaScript.

## Resultado esperado

Ao final, quero receber a URL pública do site e validar no navegador uma conversa completa com o agente, incluindo uma consulta de procedimento e a exibição de horários reais.
