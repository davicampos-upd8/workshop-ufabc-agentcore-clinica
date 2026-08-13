# Prompt — Agente e API

Quero criar, no modo spec, um agente de IA para a clínica de estética fictícia **Nova Essência**.

Esse agente será responsável por conversar com a cliente, entender sua queixa em linguagem natural, consultar a base de procedimentos da clínica, sugerir opções compatíveis, apresentar horários realmente disponíveis e registrar uma marcação.

O agente deve ser administrativo e informativo. Ele não pode diagnosticar condições médicas, prescrever tratamentos, prometer resultados ou substituir uma avaliação presencial. Quando não houver informação suficiente ou existir contraindicação, deve orientar a cliente a procurar um profissional habilitado.

## Execução obrigatória nesta fase

Esta é a fase de **provisionamento real do backend na conta AWS ativa**, e não uma fase de planejamento. Execute os comandos AWS CLI v2 e scripts boto3 necessários para criar, configurar e testar os recursos reais na conta e região da sessão. Não encerre entregando apenas código, scripts, infraestrutura como código, um plano ou instruções para execução posterior.

Nesta fase, envie os documentos ao S3, crie e sincronize a Knowledge Base, crie e carregue o DynamoDB, publique e invoque o AgentCore Harness, e provisione a Lambda e o API Gateway. Aguarde os estados necessários, registre os identificadores e URLs resultantes e faça chamadas reais de teste aos recursos provisionados. Se uma chamada AWS falhar, apresente o erro real e corrija-o; não simule sucesso.

## AgentCore obrigatório: Harness, não Runtime direto

Use especificamente o **Amazon Bedrock AgentCore Harness**. O Harness é um loop de agente gerenciado e configurável: ele cria e gerencia o Runtime subjacente. Não crie nem publique um AgentCore Runtime HTTP direto, servidor Python próprio, container próprio ou endpoints de protocolo do Runtime; não implemente manualmente `POST` para a invocação do agente.

Antes do deploy, leia `.kiro/skills/amazon-bedrock/references/agentcore-harness.md`, especialmente as seções **What It Is**, **Harness vs. Runtime** e **Deployment Workflow**. Siga o fluxo de criar o Harness, aguardar o status `READY` e invocá-lo pelo data plane com `runtimeSessionId`; a Lambda deve invocar o Harness, não um Runtime direto.

Você está autorizado a instalar, sem pedir confirmação adicional, as dependências ausentes realmente necessárias para executar, testar ou validar o projeto. Em razão do tempo curto do workshop, não crie uma suíte de testes nem instale ferramentas de teste apenas para cobertura; prefira smoke tests reais e proporcionais. Quando uma dependência for necessária, prefira um ambiente isolado do projeto e registre sua versão exata.

A única etapa posterior é a integração e publicação do `frontend/index.html`, tratada exclusivamente em `PROMPT-FRONTEND.md`.

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
- A sessão do terminal não contém valores pré-definidos de prefixo ou modelo. Antes de criar recursos, gere **uma única vez** um UUID curto com 8 caracteres hexadecimais minúsculos e defina `WORKSHOP_PREFIX` como `nova-essencia-<uuid-curto>`; reutilize exatamente esse valor em todos os nomes de recursos e artefatos gerados. Registre esse prefixo, os identificadores dos recursos e a URL da API em `deployment.json`, sem credenciais ou dados sensíveis, para a segunda fase reutilizar os mesmos recursos.
- Use obrigatoriamente `anthropic.claude-sonnet-4-6` (Claude Sonnet 4.6) como `BEDROCK_MODEL_ID`. Não apresente opções, não faça perguntas sobre o modelo e não substitua esse valor por outro.
- As credenciais exportadas pertencem ao usuário IAM provisionado para o workshop. Use essa identidade herdada do processo diretamente em todos os comandos AWS CLI v2 e scripts boto3.
- Crie as roles de serviço necessárias para os recursos provisionados, incluindo a Knowledge Base, o AgentCore Harness e a Lambda. Use nomes com `WORKSHOP_PREFIX`, configure a trust policy com o principal de serviço AWS correto e passe a role ao recurso correspondente.
- Para simplificar este workshop, as policies de permissão das roles de serviço podem usar `Action: "*"` e `Resource: "*"` quando necessário. Essa permissão ampla é exclusiva do ambiente de workshop e não deve ser tratada como padrão de produção.
- Para operações AWS, use exclusivamente AWS CLI v2 e boto3. As regras deste projeto têm precedência sobre recomendações genéricas presentes em referências técnicas.
- Não crie arquivos locais de credenciais ou configuração de acesso. Nunca grave credenciais ou valores sensíveis no código, em arquivos de configuração ou no repositório.
- Criar a base de conhecimento com Amazon Bedrock Knowledge Bases, Amazon S3 e S3 Vectors. Enviar todos os arquivos em `procedimentos/` ao bucket S3, criar a fonte de dados, iniciar o job de ingestão/sincronização e aguardar sua conclusão bem-sucedida. Não considerar a Knowledge Base pronta apenas por ter sido criada. Antes de avançar, executar uma consulta real de recuperação e confirmar que ela retorna trechos relevantes dos documentos enviados.
- Usar Amazon DynamoDB para persistir horários e reservas, inicializando os dados a partir dos arquivos em `dados/`.
- Usar escrita condicional no DynamoDB para evitar que duas clientes reservem o mesmo horário.
- Usar Titan Text Embeddings V2 para embeddings.
- Fazer o deploy do agente com Amazon Bedrock AgentCore Harness, com memória e limites explícitos de iterações, tokens e tempo de execução.
- Utilizar a skill `amazon-bedrock` como referência técnica para Bedrock Knowledge Bases e AgentCore Harness.
- Os recursos criados devem utilizar o prefixo definido em `WORKSHOP_PREFIX`.

## API para integração

Provisione **agora, nesta mesma fase**, a API HTTP que expõe o agente. A etapa posterior se refere somente a conectar e publicar o front-end, e não à criação da API.

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
