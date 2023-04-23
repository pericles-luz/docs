# API para Ressarcimento e Diárias


## Rotas de autenticação

### POST /auth/token

Envia um token de acesso para o usuário. O token é enviado por SMS ou e-mail, dependendo do valor de `meioEnvio`.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "media": 1
}
```

- `cpf`: CPF do usuário
- `media`: Meio de envio do token de acesso. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "mensagem": "texto a ser exibido ao usuário",
}
```


### POST /auth

Autentica o usuário com o token de acesso.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "token": "123456"
}
```

- `cpf`: CPF do usuário
- `token`: Token de acesso enviado ao usuário

#### JSON de retorno

```json
{
  "bearerToken": "JWT",
  "mensage": "texto a ser exibido ao usuário"
}
```

- `bearerToken`: Token de acesso para as demais rotas
- `mensage`: Texto a ser exibido ao usuário

O token de acesso é válido por 24 horas e seu payload será:

```json
{
  "exp": 1234567890,
  "iss": "sindireceita.org.br",
  "dados": {
    "cpf": "00000000191",
    "nome": "Nome do usuário",
    "unidadeSindical": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "nome": "Nome da unidade sindical"
    },
    "permissao": 1
  }
}
```

- `exp`: Data de expiração do token
- `dados`: Dados do usuário
- `permissao`: Permissão do usuário é um número `int64`. Cada bit do número representa uma permissão do sistema. As permissões são:
  - 1: Usuário - podee acessar o sistema e fazer requisições
  - 2: Aprovador - pode aprovar requisições
  - 4: Pagador - pode informar que o pagamento foi realizado

Para confirmar se um usuário tem uma permissão, basta fazer a operação `permissao & permissaoDesejada` e verificar se o resultado é diferente de zero. Por exemplo, para verificar se o usuário tem permissão de usuário, basta fazer `permissao & 1` e verificar se o resultado é diferente de zero. Em javascript, seria algo assim:

```javascript
const PERMISSAO_USUARIO = 1
if (permissao & PERMISSAO_USUARIO) {
  // usuário tem permissão de usuário
}
```

### GET /auth/certificate

Autentica o usuário com o certificado digital.

#### O JSON de retorno é o mesmo do POST /auth

## Rotas autenticadas

As rotas autenticadas devem ser enviadas com o token de acesso no header `Authorization` com o prefixo `Bearer `.

### GET /requests

Retorna resumos das últimas requisições do usuário.

#### JSON de retorno

```json
{
  "data": [{
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
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
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
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
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
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
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
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
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Os comprovantes estão anexados"
}
```

### PUT /requests/approve

Aprova uma requisição.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Favor enviar o comprovante de pagamento por email"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /requests/deny

Reprova uma requisição.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Não cabe ressarcimento de entrada em cinema"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /requests/payment

Informa que o pagamento foi realizado.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "paymentDate": "2020-01-01",
  "note": "Pagamento feito por TED"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /requests/payment/error

Informa que houve um erro no pagamento.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Conta inválida"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /requests/cancel

Cancela uma requisição.

#### JSON de entrada

```json
{
  "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
  "note": "Solicitação incorreta"
}
```

#### JSON de Retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
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
  "mensage": "texto a ser exibido ao usuário"
}
```

### POST /requests/{id}/items

Adiciona um item a uma requisição.


#### JSON de entrada - Diária

```json
{
  "type": 1,
  "dailyType": 1,
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
- `costCenterID` (obrigatório): ID do centro de custo (buscar centros de custo em `/costCenters`)

#### JSON de retorno

```json
{
  "data": {
    "requestItemID": "uuidv4"
  },
  "mensage": "texto a ser exibido ao usuário"
}
```

### POST /requests/items/attachment

Adiciona um anexo a um item de requisição.

#### JSON de entrada

```json
{
  "requestItemID": "65083027-c978-498d-974b-9ee04baf7b35",
  "file": "base64"
}
```

#### JSON de retorno

```json
{
  "data": {
    "requestItemAttachmentID": "uuidv4"
  },
  "mensage": "texto a ser exibido ao usuário"
}
```

### DELETE /requests/items/{id}

Remove um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### DELETE /requests/items/attachment/{id}

Remove um anexo de um item de requisição.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
}
```

### PUT /requests/items

Atualiza um item de requisição.

#### JSON de entrada

```json
{
  "requestItemID": "65083027-c978-498d-974b-9ee04baf7b35",
  "dailyType": 1,
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
  "mensage": "texto a ser exibido ao usuário"
}
```

### GET /requests/{id}/signing-code/{media}

Solicita o código de assinatura de uma requisição. O código será enviado ao usuário conforme o meio definido em `media`.

- `media` (obrigatório): Meio de envio do código. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "mensage": "texto a ser exibido ao usuário"
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
  "mensage": "texto a ser exibido ao usuário"
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

### GET /cities

Retorna as cidades cadastradas.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": 1,
      "name": "São Paulo",
      "state": "SP"
    },
    {
      "id": 2,
      "name": "Rio de Janeiro",
      "state": "RJ"
    }
  ]
}
```

### GET /cities/{id}

Retorna os detalhes de uma cidade.

#### JSON de retorno

```json
{
  "data": {
    "id": 1,
    "name": "São Paulo",
    "state": "SP"
  }
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

### GET /dailyTypes/{unionUnitID}

Retorna os valores de diárias cadastradas para o usuário.

- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/payerUnities`)

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "type": 1,
      "description": "Diária parcial",
      "value": 10000
    },
    {
      "id": "uuidv4",
      "type": 2,
      "description": "Diária cheia",
      "value": 20000
    }
  ]
}
```

### GET /payerUnities

Retorna as unidades pagadoras cadastradas.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "name": "Unidade pagadora 1"
    },
    {
      "id": "uuidv4",
      "name": "Unidade pagadora 2"
    }
  ]
}
```

### GET /costCenters/{id}

Retorna o centro de custo desejado

- `id` (obrigatório): ID do centro de custo

#### JSON de retorno

```json
{
  "data":
    {
      "id": 1,
      "name": "Centro de custo 1"
    }
}
```

### GET /costCenters/{unionUnitID}

Retorna os centros de custo cadastrados para o autorizador.

- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/payerUnities`)

#### JSON de retorno

```json
{
  "data": [
    {
      "id": 1,
      "name": "Centro de custo 1"
    },
    {
      "id": 2,
      "name": "Centro de custo 2"
    }
  ]
}
```