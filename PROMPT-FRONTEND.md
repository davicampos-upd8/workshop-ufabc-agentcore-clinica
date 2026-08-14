# Prompt — Integração do front-end

Quero conectar o front-end pré-construído disponível em `frontend/index.html` à API do agente da clínica Nova Essência.

O agente e a API com AWS Lambda e Amazon API Gateway já foram construídos. Nesta etapa, não recrie a Knowledge Base, o AgentCore Harness nem o backend.

## Integração esperada

- Ler `deployment.json` gerado na primeira fase e reutilizar exatamente o `WORKSHOP_PREFIX`, a URL do API Gateway e os identificadores registrados. Não gerar um novo prefixo nem recriar recursos de backend.
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

## Publicação real no Amazon S3

Esta fase deve publicar o front-end de fato na conta AWS da sessão; não encerre apenas gerando arquivos ou instruções. Crie um bucket S3 com nome baseado em `WORKSHOP_PREFIX`, configure-o para hospedagem de site estático com `index.html` como documento inicial e envie o conteúdo atualizado de `frontend/` para o bucket.

Para simplificar o workshop, torne o site público: desative no bucket as configurações de bloqueio de acesso público necessárias para aplicar uma política pública e crie uma bucket policy que permita somente leitura pública de objetos (`s3:GetObject`) em `arn:aws:s3:::<bucket>/*`. Não conceda escrita ou listagem pública. Não adicione CloudFront, domínio próprio, HTTPS, WAF ou endurecimento adicional nesta fase.

Configure ou confirme o CORS do API Gateway com `Access-Control-Allow-Origin: *`, sem credenciais, permitindo `GET`, `POST`, `OPTIONS` e o cabeçalho `Content-Type`; não peça uma origem específica.

Obtenha o endpoint público de website do S3, atualize `deployment.json` com a URL do site sem registrar segredos e valide-o de verdade: a página deve responder HTTP 200 no endpoint público e uma conversa no navegador deve chegar ao `POST /chat` e exibir a resposta do agente.

Nenhuma credencial AWS pode ser incluída no HTML ou no JavaScript.

## Resultado esperado

Ao final, quero receber a URL pública do site e validar no navegador uma conversa completa com o agente, incluindo uma consulta de procedimento e a exibição de horários reais.
