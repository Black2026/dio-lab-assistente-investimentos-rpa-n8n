# Evidências de execução — 24/10/2026

As evidências abaixo foram conferidas em capturas de tela compartilhadas durante o desenvolvimento. **As capturas de tela originais ainda não estão anexadas ao repositório.**

## Execução do Google Colab

- 10 clientes fictícios extraídos da tabela HTML.
- POST ao Webhook de teste do n8n.
- Status HTTP: **200**.
- Corpo da resposta recebido no Colab:

```json
{
  "status": "ok",
  "total_clientes": 10,
  "total_mensagens": 10,
  "envio_automatico": false
}
```

## Execução do n8n Cloud

- Webhook: 1 item de entrada, contendo a lista de clientes.
- HTTP Request: 1 item.
- Normalizar investimentos: 9 itens.
- Selecionar investimentos: 10 itens.
- Message a model: 10 itens.
- Validar mensagens: 10 itens.
- Respond to Webhook: 10 itens processados no nó, resposta JSON de confirmação.
- Autorizar envio: 10 itens no ramo **false**, 0 no ramo **true**.
- Gmail: **desativado**, sem envio automático aos clientes fictícios.

## Reprodutibilidade e segurança

- Notebook: [`rpa/extrair_clientes.ipynb`](../rpa/extrair_clientes.ipynb).
- Workflow revisado: [`n8n/workflow.json`](../n8n/workflow.json).
- A exportação publicada não inclui credenciais; configure OpenAI/Gateway após importação.
- O endereço do Webhook no notebook é um exemplo e deve ser substituído pela URL do ambiente.
- O workflow publicado mantém Gmail desativado.
- O teste confirmado utilizou a **URL de teste** do n8n; a operação contínua via URL de produção não foi validada.

## Capturas de tela a anexar (opcional para reforçar a entrega)

1. Colab mostrando total 10, HTTP 200 e resposta JSON.
2. Canvas do n8n com nós verdes e Gmail desativado.
3. Configuração do Webhook com POST e Respond to Webhook.

Antes de anexar imagens, ocultar identificadores operacionais, e-mails e qualquer informação sensível.
