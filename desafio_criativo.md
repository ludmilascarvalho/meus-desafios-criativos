Atue como especialista em n8n e gere um workflow real, importável e funcional para receber e tratar solicitações enviadas por um formulário online.

## Objetivo
Receber solicitações, validar os dados, classificá-las, registrá-las no Google Sheets e enviar e-mails à pessoa solicitante e à equipe responsável.

## Fluxo principal
1. Receber os dados por um nó Webhook do n8n.
2. Padronizar os campos e remover espaços desnecessários.
3. Validar os campos obrigatórios e o formato do e-mail.
   - Se os dados forem inválidos, não os encaminhe para classificação, planilha ou e-mails.
   - Responder ao formulário com HTTP 422 e uma mensagem indicando o que precisa ser corrigido.
4. Identificar o tipo de solicitação usando categorias e regras fáceis de editar.
   - Se nenhuma categoria corresponder, classificar como “Não classificada”.
5. Evitar duplicidades usando um identificador estável da solicitação (`submission_id`) enviado pelo formulário.
   - Consultar a planilha antes de inserir uma nova linha.
   - Se o identificador já existir, não criar outra linha nem enviar e-mails duplicados; responder que a solicitação já foi recebida.
6. Registrar no Google Sheets os dados da solicitação, sua classificação, data de recebimento e status.
7. Somente após confirmar a gravação na planilha:
   - Enviar confirmação à pessoa solicitante pelo Gmail.
   - Notificar a equipe pelo Gmail.
   - Se a solicitação estiver “Não classificada”, destacar no e-mail que a equipe precisa revisar a classificação.
8. Responder ao formulário com sucesso somente depois que a solicitação estiver registrada.

## Tratamento de erros e prevenção de perdas
- Configure tentativas automáticas nos nós que suportarem essa opção.
- Se o registro na planilha falhar, não envie confirmação de recebimento.
- Se o envio de um e-mail falhar, mantenha a solicitação registrada e sinalize a falha para permitir acompanhamento e reprocessamento.
- Use o tratamento de erros nativo do n8n. Se o Error Trigger exigir um workflow separado, forneça também esse workflow em um JSON importável separado.
- Em caso de falha, inclua o `submission_id`, o nome do workflow e a mensagem de erro no alerta à equipe, quando possível.
- Não afirme que a prevenção de duplicidades é garantida em envios simultâneos se a solução com Google Sheets não oferecer essa garantia; explique a limitação e como reduzi-la.

## Entrega
- Gere o(s) arquivo(s) JSON efetivamente importáveis pelo n8n — não entregue apenas uma descrição ou pseudocódigo.
- Use nós nativos do n8n e conexões válidas entre eles.
- Não inclua credenciais, tokens ou dados pessoais reais. Deixe credenciais, ID da planilha, nome da aba, e-mail da equipe, campos e categorias como valores claramente editáveis.
- Inclua nomes claros nos nós e notas no workflow explicando o que precisa ser configurado.
- Antes de entregar, confira a estrutura do JSON, os nomes dos nós, os parâmetros e as conexões. Não invente que o workflow foi testado no n8n se não tiver sido possível importá-lo e executá-lo.

## Depois do JSON
Apresente instruções breves para:
1. Importar o workflow no n8n.
2. Conectar as credenciais do Google Sheets e Gmail.
3. Configurar a planilha, os campos do formulário, as categorias e o e-mail da equipe.
4. Testar solicitações válidas, inválidas, não classificadas, duplicadas e casos de falha.