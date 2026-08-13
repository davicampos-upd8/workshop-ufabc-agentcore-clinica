# Prompt — Agente e API

Quero criar um agente de IA para a clínica de estética fictícia **Nova Essência**.

Esse agente será responsável por conversar com a cliente, entender sua queixa em linguagem natural, consultar a base de procedimentos da clínica, sugerir opções compatíveis, apresentar horários realmente disponíveis e registrar uma marcação.

O agente deve ser administrativo e informativo. Ele não pode diagnosticar condições médicas, prescrever tratamentos, prometer resultados ou substituir uma avaliação presencial. Quando não houver informação suficiente ou existir contraindicação, deve orientar a cliente a procurar um profissional habilitado.

## Capacidades esperadas

- Consultar os documentos em `procedimentos/` para explicar os procedimentos e relacioná-los às queixas apresentadas.
- Usar `dados/procedimentos.json` e `dados/agenda.json` como carga inicial dos dados operacionais.
- Persistir agenda e reservas em Amazon DynamoDB durante a execução.
- Retornar somente horários existentes, livres e com duração suficiente.
- Registrar a reserva com escrita condicional para impedir dupla marcação.
- Manter o contexto da conversa para que a cliente não precise repetir sua queixa, o procedimento ou o horário escolhido.
- Pedir confirmação antes de concluir uma reserva.

## Stack e definições técnicas

- Antes de abrir o Kiro CLI, a sessão do terminal terá recebido credenciais AWS exportadas em `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` e, quando aplicável, `AWS_SESSION_TOKEN`, além de `AWS_REGION` e `AWS_DEFAULT_REGION`.
- A mesma sessão também terá `WORKSHOP_PREFIX`, `WORKSHOP_SLOT` e `BEDROCK_MODEL_ID` exportadas com os valores do slot.
- As credenciais exportadas pertencem ao usuário IAM provisionado para o workshop. Use essa identidade herdada do processo diretamente em todos os comandos AWS CLI v2 e scripts boto3.
- Não procure, peça, crie, altere, assuma ou configure roles IAM; este workshop não usa ARNs de roles de execução. Não crie, carregue ou exija um arquivo `.env`; nunca grave credenciais ou valores sensíveis no código, em arquivos de configuração ou no repositório.
- Criar a base de conhecimento com Amazon Bedrock Knowledge Bases, Amazon S3 e S3 Vectors. Os arquivos em `procedimentos/` são a fonte de conhecimento.
- Usar Amazon DynamoDB para persistir horários e reservas, inicializando os dados a partir dos arquivos em `dados/`.
- Usar escrita condicional no DynamoDB para evitar que duas clientes reservem o mesmo horário.
- Usar Titan Text Embeddings V2 para embeddings.
- Fazer o deploy do agente com Amazon Bedrock AgentCore Harness, com memória e limites explícitos de iterações, tokens e tempo de execução.
- Utilizar a skill `amazon-bedrock` como referência técnica para Bedrock Knowledge Bases e AgentCore Harness.
- Os recursos criados devem utilizar o prefixo definido em `WORKSHOP_PREFIX`.

## API para integração

Também será necessário construir uma API HTTP para expor o agente ao front-end em uma etapa posterior.

A API deve usar **AWS Lambda e Amazon API Gateway**. A Lambda será responsável por receber a mensagem, invocar o AgentCore Harness com credenciais AWS e devolver a resposta. O navegador nunca deve receber credenciais AWS nem acessar o Harness diretamente.

Criar pelo menos os endpoints:

- `POST /chat`
  - Entrada: `{ "message": "texto da cliente", "sessionId": "identificador da conversa" }`
  - Saída: `{ "reply": "resposta do agente", "sessionId": "identificador da conversa" }`
- `GET /health`
  - Saída simples indicando que a API está disponível.

A API deve preservar a sessão recebida, tratar erros sem expor detalhes internos da AWS e deixar o CORS preparado para o front-end estático.

## Resultado esperado

Ao final, quero ter:

1. Os documentos enviados e ingeridos em uma Knowledge Base funcional.
2. O agente publicado no AgentCore Harness e respondendo com memória.
3. Consulta e reserva de horários funcionando sem invenção ou conflito.
4. A API Gateway com Lambda expondo o agente por HTTP.
5. Um teste completo demonstrando uma conversa desde a queixa inicial até a confirmação da reserva.

O front-end disponível em `frontend/index.html` já está pronto visualmente, mas **não deve ser alterado nem publicado nesta etapa**. A integração com ele será feita separadamente.
