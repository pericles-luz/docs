# API para solicitação de viagens

## Objetivo

O objetivo desta API é permitir que o usuário solicite passagens e hospedagem para suas viagens.

### POST /travels/requests

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

### GET /travels/requests

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

### GET /travels/requests/waitingApproval

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

### GET /travels/requests/waitingBuying

Retorna resumo das requisições que estão pendentes de compra pelo usuário.

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
    "statusName": "Aguardando compra",
  }]
}
```

### GET /travels/requests/{id}

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

### PUT /travels/requests/submit

Envia uma requisição para aprovação.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Preciso de quarto para não fumantes"
}
```

### PUT /travels/requests/approve

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

### PUT /travels/requests/deny

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

### PUT /travels/requests/buy

Informa que a compra foi realizada.

#### JSON de entrada

```json
{
  "id": "65083027-c978-498d-974b-9ee04baf7b35",
  "BuyingDate": "2020-01-01",
  "note": "Compra conforme solicitado"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /travels/requests/Buying/error

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

### PUT /travels/requests/cancel

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

### POST /travels/requests/{id}/items

Adiciona um item a uma requisição.

#### JSON de entrada

```json
{
    "type":           4,
    "originID":       1,
    "originAirportID": 1,
    "destinationID":  2,
    "destinationAirportID": 2,
    "departureDate":  "2023-05-13",
    "departureHours": "08-12",
    "returnDate":     "2023-05-14",
    "returnHours":    "08-12",
    "note":           "Teste de diária",
    "needHosting":    true,
    "needTickets":    true,
    "airModal":       true,
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
- `airModal` (obrigatório para tipo 1): Indica se a viagem será de avião
- `originAirportID` (obrigatório para tipo 1 se `airModal` for `true`): ID do aeroporto de origem (buscar aeroportos em `/airports/{state}`)
- `destinationAirportID` (obrigatório para tipo 1 se `airModal` for `true`): ID do aeroporto de destino (buscar aeroportos em `/airports/{state}`)

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c978-498d-974b-9ee04baf7b35",
    "type": 4,
    "originID": 1,
    "originName": "São Paulo",
    "originAirportID": 1,
    "originAirportName": "Aeroporto de Congonhas",
    "destinationID": 2,
    "destinationName": "Rio de Janeiro",
    "destinationAirportID": 2,
    "destinationAirportName": "Aeroporto Santos Dumont",
    "departureDate": "2023-05-13",
    "departureHours": "08-12",
    "returnDate": "2023-05-14",
    "returnHours": "08-12",
    "note": "Teste de diária",
    "needHosting": true,
    "needTickets": true,
    "airModal": true,
    "costCenterID": 1,
    "costCenterName": "Centro de custo 1",
    "status": 1,
    "statusName": "Aguardando aprovação"
  }
}
```

### PUT /travels/requests/items/{id}

Atualiza um item de uma requisição.

#### JSON de entrada

```json
{
    "type":           4,
    "originID":       1,
    "originAirportID": 1,
    "destinationID":  2,
    "destinationAirportID": 2,
    "departureDate":  "2023-05-13",
    "departureHours": "08-12",
    "returnDate":     "2023-05-14",
    "returnHours":    "08-12",
    "note":           "Teste de diária",
    "needHosting":    true,
    "needTickets":    true,
    "airModal":       true
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
    "originAirportID": 1,
    "originAirportName": "Aeroporto de Congonhas",
    "destinationID": 2,
    "destinationName": "Rio de Janeiro",
    "destinationAirportID": 2,
    "destinationAirportName": "Aeroporto Santos Dumont",
    "departureDate": "2023-05-13",
    "departureHours": "08-12",
    "returnDate": "2023-05-14",
    "returnHours": "08-12",
    "note": "Teste de diária",
    "needHosting": true,
    "needTickets": true,
    "airModal": true,
    "status": 1,
    "statusName": "Aguardando aprovação"
  }
}
```

### DELETE /travels/requests/items/{id}

Remove um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### POST /travels/requests/items/{id}/attachments/{type}

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
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /travels/requests/items/{id}/attachments

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

### DELETE /travels/requests/items/attachments/{id}

Remove um anexo de um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /file/{id}

Retorna um arquivo em seu formato original, mas deve ser enviado o header `Authorization` com o token de acesso.

### POST /travels/requests/items/{id}/refund

Gera a requisição de diárias e auxílio deslocamento automaticamente, a partir de um item de requisição de viagem.

#### JSON de entrada

```json
{
  "accountID": "65083027-c978-498d-974b-9ee04baf7b35",
  "subType": 1
}
```

- `accountID` (obrigatório): ID da conta bancária (buscar contas em `/accounts`)
- `subType` (obrigatório): Tipo de diária a ser considerada (buscar tipos em `/dailyTypes`)

O retorno é 204