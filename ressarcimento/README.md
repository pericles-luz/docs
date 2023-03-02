# API para Ressarcimento e Diárias


## Rotas de autenticação

### POST /autenticar/token

Envia um token de acesso para o usuário. O token é enviado por SMS ou e-mail, dependendo do valor de `meioEnvio`.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "meioEnvio": 1
}
```

- `cpf`: CPF do usuário
- `meioEnvio`: Meio de envio do token de acesso. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "mensagem": "texto a ser exibido ao usuário",
}
```


### POST /autenticar

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
  "mensagem": "texto a ser exibido ao usuário"
}
```

- `bearerToken`: Token de acesso para as demais rotas
- `mensagem`: Texto a ser exibido ao usuário

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
    }
  }
}
```

- `exp`: Data de expiração do token
- `dados`: Dados do usuário

### GET /autenticar/certificado

Autentica o usuário com o certificado digital.

#### O JSON de retorno é o mesmo do POST /autenticar

## Rotas autenticadas

As rotas autenticadas devem ser enviadas com o token de acesso no header `Authorization` com o prefixo `Bearer `.

### GET /requests

Retorna resumos das últimas requisições do usuário.

#### JSON de retorno

```json
{
  "data": [{
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitName": "Nome da unidade sindical",
    "statusName": "Aguardando aprovação",
    "ammount": 100.00
  }]
}
```

### GET /requests/waiting-approval

Retorna resumos das requisições que estão pendentes de aprovação do usuário.

#### JSON de retorno

```json
{
  "data": [{
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitName": "Nome da unidade sindical",
    "statusName": "Aguardando aprovação",
    "ammount": 100.00
  }]
}
```

### GET /requests/wating-payment

Retorna resumo das requisições que estão pendentes de pagamento pelo usuário.

#### JSON de retorno

```json
{
  "data": [{
    "requestID": "65083027-c978-498d-974b-9ee04baf7b35",
    "requesterName": "Nome do solicitante",
    "unionUnitName": "Nome da unidade sindical",
    "statusName": "Aguardando pagamento",
    "ammount": 100.00
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
    "ammount": 100.00
  }
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

### PUT /requests/{id}/payment/error

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

### PUT /requests/{id}/cancel

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
- `dailyType` (obrigatório para tipo 1): Tipo de diária a ser considerada
- `originID` (obrigatório para tipo 1 e 3): ID da cidade de origem
- `destinationID` (obrigatório para tipo 1 e 3): ID da cidade de destino
- `start` (obrigatório): Data de início ou de acontecimento do item
- `finish` (obrigatório para tipo 1): Data de término do item
- `description` (obrigatório para 1 e 2): Descrição do item
- `amount` (obrigatório para tipo 2): Valor do ressarcimento
- `costCenterID` (obrigatório): ID do centro de custo

#### JSON de retorno

```json
{
  "data": {
    "requestItemID": "uuidv4"
  },
  "mensage": "texto a ser exibido ao usuário"
}
```

#### JSON de entrada - Auxílio deslocamento

```json
{
  "type": 3,
  "originID": 1,
  "destinationID": 2,
  "start": "2017-01-01",
  "description": "trabalho parlamentar",
  "costCenterID": 1
}
```

#### JSON de entrada - Ressarcimento

```json
{
  "type": 2,
  "originID": 1,
  "start": "2017-01-01",
  "description": "estacionamento",
  "costCenterID": 1,
  "amount": 10.00
}
```

### POST /requests/items/{id}/attachment

Adiciona um anexo a um item de requisição.

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

- `dailyType` (obrigatório para tipo 1): Tipo de diária a ser considerada
- `originID` (obrigatório para tipo 1 e 3): ID da cidade de origem
- `destinationID` (obrigatório para tipo 1 e 3): ID da cidade de destino
- `start` (obrigatório): Data de início ou de acontecimento do item
- `finish` (obrigatório para tipo 1): Data de término do item
- `description` (obrigatório para 1 e 2): Descrição do item
- `amount` (obrigatório para tipo 2): Valor do ressarcimento
- `costCenterID` (obrigatório): ID do centro de custo


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

### GET /cities

Retorna as cidades cadastradas.

#### JSON de retorno

```json
{
  "data": [
    {
      "cityID": 1,
      "name": "São Paulo",
      "state": "SP"
    },
    {
      "cityID": 2,
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
    "cityID": 1,
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
    "accountID": "uuidv4",
    "bankName": "Banco do Brasil",
    "number": "000001",
  }]
}
```

### GET /accounts/{id}

Retorna os detalhes de uma conta corrente.

#### JSON de retorno

```json
{
  "data": {
    "accountID": "uuidv4",
    "bankName": "Banco do Brasil",
    "bank": "001",
    "branch": "0001",
    "branchDV": "0",
    "number": "000001",
    "digit": "0"
  }
}
```

### GET /daily

Retorna os valores de diárias cadastradas para o usuário.

#### JSON de retorno

```json
{
  "data": [
    {
      "dailyID": 1,
      "type": 1,
      "name": "Diária parcial",
      "value": 100.00
    },
    {
      "dailyID": 2,
      "type": 2,
      "name": "Diária integral",
      "value": 200.00
    }
  ]
}
```

### GET /payer-unity

Retorna as unidades pagadoras cadastradas.

#### JSON de retorno

```json
{
  "data": [
    {
      "unionUnityID": "uuidv4",
      "name": "Unidade pagadora 1"
    },
    {
      "unionUnityID": "uuidv4",
      "name": "Unidade pagadora 2"
    }
  ]
}
```

### GET /cost-center

Retorna os centros de custo cadastrados para o autorizador.

#### JSON de retorno

```json
{
  "data": [
    {
      "costCenterID": 1,
      "name": "Centro de custo 1"
    },
    {
      "costCenterID": 2,
      "name": "Centro de custo 2"
    }
  ]
}
```