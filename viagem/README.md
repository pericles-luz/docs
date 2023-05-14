# API para solicitação de viagens

## Objetivo

O objetivo desta API é permitir que o usuário solicite passagens e hospedagem para suas viagens.

### POST /travel/requests

Cria uma requisição.

#### JSON de entrada

```json
{
  "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35",
  "requesterID": "65083027-c978-498d-974b-9ee04baf7b35",
}
```

- `unionUnitID` (obrigatório): ID da unidade sindical
- `requesterID` (opcional): ID do solicitante. Se não for informado, será considerado o ID do usuário autenticado.

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
  },
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /travel/requests

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
    "statusName": "Aguardando pagamento"
  }]
}
```

### GET /travel/requests/waitingApproval

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
    "statusName": "Aguardando pagamento"
  }]
}
```

### GET /travel/requests/waitingPayment

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
  }]
}
```

### GET /travel/requests/{id}

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
  }
}
```

### PUT /travel/requests/submit

Envia uma requisição para aprovação.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Preciso de quarto para não fumantes"
}
```

### PUT /travel/requests/approve

Aprova uma requisição.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Aprovado por ser viagem a trabalho sindical"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /travel/requests/deny

Reprova uma requisição.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Os valores de passagem estão muito altos"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /travel/requests/buy

Informa que a compra foi realizada.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "paymentDate": "2020-01-01",
  "note": "Compra conforme solicitado"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /travel/requests/payment/error

Informa que houve um erro no pagamento.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Voos lotados"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /travel/requests/cancel

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
  "mensage": "texto a ser exibido ao usuário"
}
```

### POST /travel/requests/{id}/items

Adiciona um item a uma requisição.

#### JSON de entrada

```json
{
    "type":           4,
    "originID":       1,
    "destinationID":  2,
    "departureDate":  "2023-05-13",
    "departureHours": "08-12",
    "returnDate":     "2023-05-14",
    "returnHours":    "08-12",
    "note":           "Teste de diária",
    "needHosting":    true,
    "needTickets":    true,
    "costCenterID":   1,
}
```

- `type` - Tipo de item. 4 - Viagem
- `originID` - ID da cidade de origem (buscar cidades em `/cities`)
- `destinationID` - ID da cidade de destino (buscar cidades em `/cities`)
- `departureDate` - Data de ida
- `departureHours` - Horários possíveis de ida
- `returnDate` - Data de volta
- `returnHours` - Horários possíveis de volta
- `note` - Observações
- `needHosting` - Necessita de hospedagem
- `needTickets` - Necessita de passagens
- `costCenterID` - ID do centro de custo (buscar centros de custo em `/costCenters/{unionUnitID}`)

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "type": 4,
    "originID": 1,
    "originName": "São Paulo",
    "destinationID": 2,
    "destinationName": "Rio de Janeiro",
    "departureDate": "2023-05-13",
    "departureHours": "08-12",
    "returnDate": "2023-05-14",
    "returnHours": "08-12",
    "note": "Teste de diária",
    "needHosting": true,
    "needTickets": true,
    "costCenterID": 1,
    "costCenterName": "Centro de custo 1",
    "status": 1,
    "statusName": "Aguardando aprovação"
  }
}
```

### PUT /travel/requests/items/{id}

Atualiza um item de uma requisição.

#### JSON de entrada

```json
{
    "type":           4,
    "originID":       1,
    "destinationID":  2,
    "departureDate":  "2023-05-13",
    "departureHours": "08-12",
    "returnDate":     "2023-05-14",
    "returnHours":    "08-12",
    "note":           "Teste de diária",
    "needHosting":    true,
    "needTickets":    true,
}
```

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "type": 4,
    "originID": 1,
    "originName": "São Paulo",
    "destinationID": 2,
    "destinationName": "Rio de Janeiro",
    "departureDate": "2023-05-13",
    "departureHours": "08-12",
    "returnDate": "2023-05-14",
    "returnHours": "08-12",
    "note": "Teste de diária",
    "needHosting": true,
    "needTickets": true,
    "status": 1,
    "statusName": "Aguardando aprovação"
  }
}
```

### DELETE /travel/requests/items/{id}

Remove um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### POST /travel/requests/items/{id}/attachments

Adiciona um anexo a um item de requisição. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64.

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
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /travel/requests/items/{id}/attachments

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
  "mensage": "texto a ser exibido ao usuário"
}
```

### DELETE /travel/requests/items/attachments/{id}

Remove um anexo de um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /file/{id}

Retorna um arquivo em seu formato original, mas deve ser enviado o header `Authorization` com o token de acesso.
