# API para Ressarcimento e Diárias

## Rotas autenticadas

As rotas autenticadas devem ser enviadas com o token de acesso no header `Authorization` com o prefixo `Bearer `.

### GET /requests

Retorna resumos das últimas requisições do usuário.

#### JSON de retorno

```json
{
  "data": [{
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
    "unionUnitName": "Nome da unidade sindical",
    "status": 2,
    "statusName": "Aguardando pagamento",
    "amount": 100.00
  }]
}
```

### GET /requests/waitingApproval

Retorna resumos das requisições que estão pendentes de aprovação do usuário.

#### JSON de retorno

```json
{
  "data": [{
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
    "unionUnitName": "Nome da unidade sindical",
    "status": 2,
    "statusName": "Aguardando pagamento",
    "amount": 10000
  }]
}
```

### GET /requests/waitingPayment

Retorna resumo das requisições que estão pendentes de pagamento pelo usuário.

#### JSON de retorno

```json
{
  "data": [{
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
    "unionUnitName": "Nome da unidade sindical",
    "status": 3,
    "statusName": "Aguardando pagamento",
    "amount": 10000
  }]
}
```

### GET /requests/{id}

Retorna os detalhes de uma requisição.

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
    "unionUnitName": "Nome da unidade sindical",
    "status": 2,
    "statusName": "Aguardando pagamento",
    "amount": 10000
  }
}
```

### PUT /requests/submit

Envia uma requisição para aprovação.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Os comprovantes estão anexados"
}
```

### PUT /requests/approve

Aprova uma requisição.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Favor enviar o comprovante de pagamento por email"
}
```

#### JSON de Retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/deny

Reprova uma requisição.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Não cabe ressarcimento de entrada em cinema"
}
```

#### JSON de Retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/pay

Informa que o pagamento foi realizado.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "paymentDate": "2020-01-01",
  "note": "Pagamento feito por TED"
}
```

#### JSON de Retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/payment/error

Informa que houve um erro no pagamento.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Conta inválida"
}
```

#### JSON de Retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/cancel

Cancela uma requisição.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Solicitação incorreta"
}
```

#### JSON de Retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### POST /requests

Cria uma requisição.

#### JSON de entrada

```json
{
  "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
  "accountID": "65083027-c978-498d-974b-9ee04baf7b35",
  "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
}
```

- `unionUnitID` (obrigatório): ID da unidade sindical
- `accountID` (obrigatório): ID da conta bancária
- `requesterID` (opcional): ID do solicitante. Se não for informado, será considerado o ID do usuário autenticado.

#### JSON de retorno

```json
{
  "data": {
    "requestID": "uuidv4"
  },
  "message": "texto a ser exibido ao usuário"
}
```

### POST /requests/{id}/items

Adiciona um item a uma requisição.


#### JSON de entrada - Diária

```json
{
  "type": 1,
  "subType": 1,
  "originID": 1,
  "destinationID": 2,
  "start": "2017-01-01",
  "finish": "2017-01-02",
  "description": "trabalho parlamentar",
  "amount": 10.00,
  "costCenterID": 1
}
```

#### JSON de entrada - Ressarcimento

```json
{
  "type": 2,
  "originID": 1,
  "start": "2017-01-01",
  "description": "despesa com correios",
  "amount": 10.00,
  "costCenterID": 1
}
```

#### JSON de entrada - Auxílio Deslocamento

```json
{
  "type": 3,
  "originID": 1,
  "destinationID": 2,
  "start": "2017-01-01",
  "costCenterID": 1
}
```
- `type` (obrigatório): Tipo de item. 1 para diária, 2 para ressarcimento, 3 para auxílio deslocamento.
- `dailyType` (obrigatório para tipo 1): Tipo de diária a ser considerada (buscar tipos em `/dailyTypes`)
- `originID` (obrigatório para tipo 1 e 3): ID da cidade de origem (buscar cidades em `/cities`)
- `destinationID` (obrigatório para tipo 1 e 3): ID da cidade de destino (buscar cidades em `/cities`)
- `start` (obrigatório): Data de início ou de acontecimento do item
- `finish` (obrigatório para tipo 1): Data de término do item
- `description` (obrigatório para 1 e 2): Descrição do item
- `amount` (obrigatório para tipo 2): Valor do ressarcimento
- `costCenterID` (obrigatório): ID do centro de custo (buscar centros de custo em `/costCenters/{unionUnitID}`)

#### JSON de retorno

```json
{
  "data": {
    "requestItemID": "uuidv4"
  },
  "message": "texto a ser exibido ao usuário"
}
```

### POST /requests/items/{id}/attachments/{type}

Adiciona um anexo a um item de requisição. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `1`: Comprovante(nota fiscal ou recibo), usado em ressarcimentos
  - `2`: Relatório(o de viagem é usado em diárias)
  - `3`: Voucher, usado em viagens
  - `4`: Sugestão de passagens, usado em viagens

#### payload de entrada

```
data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAAEnQAABJ0Ad5mH3gAAAAMSURBVBhXY3growIAAycBLhVrvukAAAAASUVORK5CYII=
```

#### JSON de retorno

```json
{
  "data": {
    "attachmentID": "uuidv4",
    "hash": "hash em sha256 do arquivo",
    "description": "descrição do arquivo",
    "createdAt": "2017-12-25 00:00:00",
    "size": 123456
  },
  "message": "texto a ser exibido ao usuário"
}
```

### GET /requests/items/{id}/attachments

Retorna os anexos de um item de requisição.

#### JSON de retorno

```json
{
  "data": [
    {
      "attachmentID": "uuidv4",
      "hash": "hash em sha256 do arquivo",
      "description": "descrição do arquivo",
      "createdAt": "2017-12-25 00:00:00",
      "size": 123456
    }
  ],
  "message": "texto a ser exibido ao usuário"
}
```

### DELETE /requests/items/{id}

Remove um item de requisição.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### DELETE /requests/items/attachments/{id}

Remove um anexo de um item de requisição.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/items

Atualiza um item de requisição.

#### JSON de entrada

```json
{
  "requestItemID": "65083027-c978-498d-974b-9ee04baf7b35",
  "subType": 1,
  "originID": 1,
  "destinationID": 2,
  "start": "2017-01-01",
  "finish": "2017-01-02",
  "description": "trabalho parlamentar",
  "amount": 10.00,
  "costCenterID": 1
}
```

- `dailyType` (obrigatório para tipo 1): Tipo de diária a ser considerada (buscar tipos em `/dailyTypes`)
- `originID` (obrigatório para tipo 1 e 3): ID da cidade de origem (buscar cidades em `/cities`)
- `destinationID` (obrigatório para tipo 1 e 3): ID da cidade de destino (buscar cidades em `/cities`)
- `start` (obrigatório): Data de início ou de acontecimento do item
- `finish` (obrigatório para tipo 1): Data de término do item
- `description` (obrigatório para 1 e 2): Descrição do item
- `amount` (obrigatório para tipo 2): Valor do ressarcimento
- `costCenterID` (obrigatório): ID do centro de custo (buscar centros de custo em `/costCenters`)


#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### GET /requests/{id}/signing-code/{media}

Solicita o código de assinatura de uma requisição. O código será enviado ao usuário conforme o meio definido em `media`.

- `media` (obrigatório): Meio de envio do código. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### PUT /requests/sign

Assina uma requisição.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "signature": "123456"
}
```

- `requestID` (obrigatório): ID da requisição
- `signature` (obrigatório): Assinatura enviada ao usuário

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

### GET /requests/{id}/history

Retorna o histórico de uma requisição.

#### JSON de retorno

```json
{
  "data": [
    {
      "requestHistoryID": "uuidv4",
      "requestID": "uuidv4",
      "dateTime": "2020-02-20 20:02:00",
      "action": "Ação realizada",
      "notes": "Observações",
      "actor": "Nome do usuário que realizou a ação"
    }
  ]
}
```

### GET /accounts

Retorna as contas correntes cadastradas para o usuário.

#### JSON de retorno

```json
{
  "data": [{
			"account": "567876",
			"accountDV": "9",
			"accountID": "uuidv4",
			"bank": "001",
			"branch": "3456",
			"branchDV": "3",
			"type": 1
		}]
}
```

### POST /accounts

Grava uma conta corrente.

#### JSON de retorno

```json
{
  "data": {
			"account": "567876",
			"accountDV": "9",
			"accountID": "uuidv4",
			"bank": "001",
			"branch": "3456",
			"branchDV": "3",
			"type": 1
		}
}
```

### GET /file/{id}

Retorna um arquivo em seu formato original, mas deve ser enviado o header `Authorization` com o token de acesso.
