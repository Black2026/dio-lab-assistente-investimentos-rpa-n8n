# Documentação técnica — Assistente de Investimentos

## Objetivo e escopo
Demonstrar um pipeline educacional: Python lê uma tabela HTML, envia clientes ao n8n; n8n consulta CSV, seleciona produto por perfil e investimento mínimo, solicita texto à OpenAI, valida a resposta e controla o envio de e-mails. Dados e mensagens são exemplos didáticos, não aconselhamento financeiro.

## Fontes e componentes
- HTML de clientes: `https://digitalinnovationone.github.io/dio-lab-assistente-investimentos-rpa-n8n/`
- CSV de investimentos: `https://digitalinnovationone.github.io/dio-lab-assistente-investimentos-rpa-n8n/data.csv`
- Python: `requests`, `beautifulsoup4`; seletor `#clientes tbody tr`; campos `nome`, `email`, `saldo`, `perfil`.
- Webhook: POST `/Clientes`, recebendo `body.clientes` como array.
- HTTP Request: GET do CSV; nó Code normaliza 9 linhas observadas.
- Seleção: filtra por igualdade de perfil e valor mínimo até o saldo; seleciona menor investimento mínimo entre elegíveis. Não realiza suitability completo.
- OpenAI: operação `Message a Model`; testes usaram Gateway credits e GPT-4.1.
- Validação: extrai texto em `output[0].content[*].text`, valida e-mail e produz `aprovado_para_envio: false`.
- IF: somente `true` permitiria Gmail. Revisão mantém Gmail desativado.
- Respond to Webhook: resposta HTTP deve ser independente do ramo Gmail.

## Diagnóstico da resposta vazia
No JSON originalmente exportado, `Respond to Webhook` estava depois do Gmail. Como todos os itens tinham `aprovado_para_envio: false`, nenhum chegava ao Gmail nem ao nó de resposta. O Colab mostrava HTTP 200 com corpo de 0 bytes. Uma revisão foi preparada com a resposta separada do envio. **O teste de desenvolvimento no n8n Cloud foi concluído com sucesso**, retornando HTTP 200 e JSON não vazio ao Colab.

## Evidências de desenvolvimento
1. Colab: 10 clientes extraídos, status 200; resposta vazia no fluxo original.
2. n8n: 9 itens normalizados e 10 itens selecionados.
3. OpenAI: 10 respostas no output.
4. Validar mensagens: 10 itens; IF: FALSE 10, TRUE 0.
5. Gmail: teste separado com 1 item e nós verdes; chegada à caixa postal não confirmada.

## Como reproduzir o teste final
1. Importar o workflow revisado como novo workflow, preservando o original.
2. Conferir URLs de teste e produção no Webhook; não editar o endereço gerado pelo n8n manualmente.
3. Selecionar uma credencial válida no OpenAI; verificar disponibilidade/saldo de Gateway credits.
4. Executar `Listen for test event` e o Colab com a URL correspondente.
5. Verificar 10 clientes, 9 investimentos, 10 mensagens, 10 validações e retorno HTTP JSON não vazio.
6. Não habilitar Gmail para os clientes. Qualquer teste de envio deve usar destinatário próprio, dados fictícios e autorização explícita.

## Riscos e limitações
- Correspondência de respostas por posição: pressupõe que o modelo devolva itens na mesma ordem; ainda requer mecanismo mais robusto antes de envio real.
- Informações de rentabilidade são textos de fonte externa, sem normalização de prazos, taxas e indexadores; a IA pode introduzir afirmações imprecisas.
- Filtro de perfil e saldo é simplificado, não avalia liquidez, vencimento, objetivos, riscos específicos ou adequação regulatória.
- Dados fictícios do laboratório não autorizam mensagens para destinatários externos.
- Credenciais não devem ser publicadas; inspecionar exports, outputs, notebooks e capturas de tela antes do commit.
- O notebook publicado em `rpa/extrair_clientes.ipynb` contém envio HTTP, diagnóstico e URL de Webhook parametrizável. O endereço operacional deve ser preenchido pelo executor.

## Checklist de entrega
- [x] Repositório existente e fontes HTML/CSV disponíveis.
- [x] Documentação técnica e README atualizados.
- [x] Testes separados de extração, IA, validação e Gmail demonstrados.
- [x] Publicar notebook atualizado e sanitizado.
- [x] Importar e testar a lógica revisada no n8n Cloud e publicar JSON sem credenciais em `n8n/workflow.json`.
- [x] Confirmar resposta HTTP JSON no Colab: `{'status':'ok','total_clientes':10,'total_mensagens':10,'envio_automatico':False}`.
- [ ] Adicionar capturas de tela sanitizadas em `evidencias/`.
- [ ] Revisar links e entregar URL pública do repositório.
