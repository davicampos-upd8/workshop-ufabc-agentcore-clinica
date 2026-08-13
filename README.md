# Nova Essência — hands-on UFABC

Seed para construir um agente de atendimento de uma clínica de estética fictícia com Kiro CLI v3, Amazon Bedrock e AgentCore.

## Roteiro do workshop

1. **Antes de abrir o Kiro CLI**, exporte no mesmo terminal as credenciais e a região AWS fornecidas para o workshop:
   ```bash
   export AWS_ACCESS_KEY_ID='...'
   export AWS_SECRET_ACCESS_KEY='...'
   export AWS_SESSION_TOKEN='...' # somente para credenciais temporárias
   export AWS_REGION='us-east-1'
   export AWS_DEFAULT_REGION="$AWS_REGION"
   ```
   As credenciais pertencem ao usuário IAM provisionado para o workshop. Não registre credenciais, tokens ou dados sensíveis no repositório.

2. Inicie o Kiro CLI v3 com confiança nas ferramentas:
   ```bash
   kiro-cli --v3 chat --trust-all-tools
   ```

3. No Kiro CLI, inicie uma conversa e abra o seletor de modelo:
   ```text
   /chat new
   /model
   ```
   No seletor interativo, escolha **GPT 5.6 Terra**.

4. Na mesma conversa, envie esta solicitação ao Kiro:
   ```text
   Baixe o seed do workshop, entre no diretório do projeto e use-o como contexto desta sessão:
   https://github.com/davicampos-upd8/workshop-ufabc-agentcore-clinica

   Se o Git não estiver disponível, baixe o conteúdo do repositório por outro método disponível e prepare o diretório do projeto da mesma forma.
   ```

5. Depois que o Kiro confirmar que está no diretório do seed, crie a Spec:
   ```text
   /spec new agente-clinica
   ```
   Em seguida, envie o conteúdo de [`PROMPT-AGENTE.md`](PROMPT-AGENTE.md). Essa primeira fase deve provisionar e testar os recursos reais de backend na conta AWS; não é apenas uma geração de código ou plano.

6. Depois que o agente e a API estiverem funcionando, abra uma nova conversa e envie o conteúdo de [`PROMPT-FRONTEND.md`](PROMPT-FRONTEND.md) para integrar e publicar o front-end.

> O modelo do Kiro nesta sessão é **GPT 5.6 Terra**. O modelo a ser usado pelo agente implantado no Amazon Bedrock continua definido no prompt como **Claude Sonnet 4.6**.

## Arquivos principais

- `PROMPT-AGENTE.md` — agente, base de conhecimento, agenda, Harness e API.
- `PROMPT-FRONTEND.md` — integração e publicação do front-end.
- `.kiro/steering/workshop.md` — contexto permanente do projeto.
- `procedimentos/` — documentos da base de conhecimento.
- `dados/` — procedimentos, agenda e marcações fictícias.
- `frontend/index.html` — interface pronta para integração.
