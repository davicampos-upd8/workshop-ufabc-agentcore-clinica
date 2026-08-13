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

A skill local `amazon-bedrock` é a referência técnica para os recursos de Bedrock e AgentCore.

## Convenções do ambiente

- As credenciais AWS já estarão configuradas na sessão do terminal.
- O arquivo `.env` contém a região, o prefixo dos recursos, o modelo aprovado e as ARNs das roles de execução.
- Todos os recursos devem usar `WORKSHOP_PREFIX` em seus nomes.
- A aplicação e suas mensagens devem estar em português do Brasil.
