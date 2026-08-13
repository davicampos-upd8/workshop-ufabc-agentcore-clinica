---
inclusion: always
---

# Contexto do projeto

Este projeto faz parte de um workshop para alunos da UFABC (Universidade Federal do ABC) e implementa um agente de atendimento para a clínica de estética fictícia **Nova Essência**.

A base inicial já contém:

- `procedimentos/`: textos informativos utilizados como fonte da base de conhecimento;
- `dados/procedimentos.json` e `dados/agenda.json`: carga inicial dos dados operacionais;
- `dados/marcacoes.json`: exemplo do formato de uma reserva;
- `frontend/index.html`: interface de chat pré-construída.

Todos os dados são fictícios e destinados exclusivamente ao workshop.

## Stack do projeto

- Python, boto3 e AWS CLI v2;
- Amazon S3 e S3 Vectors;
- Amazon DynamoDB para agenda e reservas;
- Amazon Bedrock Knowledge Bases com Titan Text Embeddings V2;
- Amazon Bedrock AgentCore Harness;
- AWS Lambda e Amazon API Gateway para a API HTTP;
- Amazon S3 para a publicação do front-end estático.

A skill local `amazon-bedrock` é uma referência técnica para Bedrock e AgentCore. As regras deste projeto têm precedência sobre qualquer recomendação genérica dessa skill.

## Execução da primeira fase

O primeiro prompt exige provisionamento real na conta AWS da sessão. O Kiro deve usar exclusivamente AWS CLI v2 e boto3 para criar, configurar e testar S3, Knowledge Base, S3 Vectors, DynamoDB, AgentCore Harness, Lambda e API Gateway; não deve apenas gerar código, scripts, IaC, plano ou instruções futuras. Deve aguardar os recursos ficarem prontos, validar chamadas reais e registrar em `deployment.json`, sem dados sensíveis, o prefixo, os identificadores dos recursos e a URL da API. Somente a integração e a publicação de `frontend/index.html` pertencem à segunda fase.

## Escopo e validação do workshop

O workshop é curto. Priorize o menor recorte funcional capaz de demonstrar o fluxo completo, em vez de ampliar a arquitetura ou adicionar abstrações não necessárias. Valide cada entrega com smoke tests reais e proporcionais — por exemplo, chamadas à API, invocação do agente ou consulta aos recursos criados.

Não crie uma suíte de testes, framework de testes ou dependências como `pytest` apenas para cobertura. Use uma ferramenta de teste já existente ou instale uma quando ela for realmente necessária para diagnosticar ou validar um comportamento específico.

## Convenções do ambiente

- Antes de abrir o Kiro CLI, as credenciais AWS (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` e, quando aplicável, `AWS_SESSION_TOKEN`) e a região (`AWS_REGION` e `AWS_DEFAULT_REGION`) já estarão exportadas na sessão do terminal.
- Antes de criar recursos, gerar **uma única vez** um UUID curto com 8 caracteres hexadecimais minúsculos e definir `WORKSHOP_PREFIX` como `nova-essencia-<uuid-curto>`. Reutilizar exatamente esse valor em todos os nomes de recursos e artefatos gerados; não perguntar por prefixo.
- Usar obrigatoriamente `anthropic.claude-sonnet-4-6` (Claude Sonnet 4.6) como `BEDROCK_MODEL_ID`; não perguntar pelo modelo nem oferecer alternativas.
- As credenciais exportadas pertencem ao usuário IAM provisionado para o workshop. O Kiro CLI e os subprocessos que ele executar devem usar essa identidade herdada do ambiente diretamente.
- O Kiro pode instalar, sem pedir confirmação adicional, dependências ausentes necessárias para executar, testar ou validar o projeto. Prefira ambiente isolado do projeto e versões exatas para dependências adicionadas.
- Não criar arquivos locais de credenciais ou configuração de acesso e não alterar a configuração de identidade ou acesso da conta. Nunca persistir credenciais ou dados sensíveis em arquivos ou código.
- Para a API Gateway deste workshop, configurar CORS com `Access-Control-Allow-Origin: *`, sem credenciais, permitindo `GET`, `POST`, `OPTIONS` e o cabeçalho `Content-Type`. Não pedir uma origem específica nem condicionar testes E2E a um domínio publicado.
- Todos os recursos devem usar `WORKSHOP_PREFIX` em seus nomes.
- A aplicação e suas mensagens devem estar em português do Brasil.
