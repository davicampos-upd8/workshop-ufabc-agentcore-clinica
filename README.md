# Nova Essência — hands-on UFABC

Seed para construir um agente de atendimento de uma clínica de estética fictícia com Kiro CLI v3, Amazon Bedrock e AgentCore.

## Como usar

1. **Antes de abrir o Kiro CLI**, exporte no mesmo terminal as credenciais AWS e os valores do seu slot entregues no workshop. Exemplo:
   ```bash
   export AWS_ACCESS_KEY_ID='...'
   export AWS_SECRET_ACCESS_KEY='...'
   export AWS_SESSION_TOKEN='...' # somente para credenciais temporárias
   export AWS_REGION='us-east-1'
   export AWS_DEFAULT_REGION="$AWS_REGION"
   ```
   Essas credenciais pertencem ao usuário IAM provisionado para o workshop. O Kiro CLI e os comandos que ele executar herdarão essas variáveis e devem usar esse usuário diretamente. O agente gerará uma vez o prefixo `nova-essencia-<uuid-curto>` e usará o modelo fixo `anthropic.claude-sonnet-4-6` (Claude Sonnet 4.6); não há escolha manual desses valores. **Não crie nem use um arquivo `.env`**, não informe ARNs de roles e nunca registre credenciais no repositório.
2. Inicie o Kiro CLI v3:
   ```bash
   kiro-cli --v3
   ```
3. Crie uma Spec e envie o conteúdo de [`PROMPT-AGENTE.md`](PROMPT-AGENTE.md). Esse primeiro prompt deve provisionar e testar os recursos reais de backend na sua conta AWS; não é apenas uma geração de código ou plano.
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
