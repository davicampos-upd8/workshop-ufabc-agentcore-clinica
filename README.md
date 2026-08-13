# Nova Essência — hands-on UFABC

Seed para construir um agente de atendimento de uma clínica de estética fictícia com Kiro CLI v3, Amazon Bedrock e AgentCore.

## Como usar

1. Configure as credenciais AWS no terminal e copie `.env.example` para `.env` com os valores do seu ambiente.
2. Inicie o Kiro CLI v3:
   ```bash
   kiro-cli --v3
   ```
3. Crie uma Spec e envie o conteúdo de [`PROMPT-AGENTE.md`](PROMPT-AGENTE.md):
   ```text
   /spec new clinica-agente
   ```
4. Depois que o agente e a API estiverem funcionando, abra uma conversa no Chat e envie o conteúdo de [`PROMPT-FRONTEND.md`](PROMPT-FRONTEND.md).

## Arquivos principais

- `PROMPT-AGENTE.md` — agente, base de conhecimento, agenda, Harness e API.
- `PROMPT-FRONTEND.md` — integração e publicação do front-end.
- `.kiro/steering/workshop.md` — contexto permanente do projeto.
- `procedimentos/` — documentos da base de conhecimento.
- `dados/` — procedimentos, agenda e marcações fictícias.
- `frontend/index.html` — interface pronta para integração.
