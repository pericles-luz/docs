# API para AGNU


## Rotas de autenticação

## Rotas autenticadas

As rotas autenticadas devem ser enviadas com o token de acesso no header `Authorization` com o prefixo `Bearer `.

### GET /agnu/opened

Retorna a lista de AGNU abertas.

#### JSON de retorno

```json

{
  "message": "texto a ser exibido ao usuário",
  "data":   [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "name": "Nome da AGNU",
    "start": "2019-01-01 12:34:45",
    "end": "2019-01-03 12:34:45",
    "unionUnit": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "name": "Nome da unidade sindical"
    }
  }]
}

```

### GET /agnu/{id}

Retorna os dados da AGNU.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": {
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "name": "Nome da AGNU",
    "startDate": "2019-01-01",
    "finishDate": "2019-01-03",
    "startHour": "05",
    "finishHour": "23",
    "unionUnit": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "name": "Nome da unidade sindical"
    }
  }
}
```

### GET /agnu

Retorna a lista de AGNU existentes.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "name": "Nome da AGNU",
    "startDate": "2019-01-01",
    "finishDate": "2019-01-03",
    "startHour": "05",
    "finishHour": "23",
    "unionUnit": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "name": "Nome da unidade sindical"
    }
  },
  {
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "name": "Nome da AGNU",
    "startDate": "2019-01-01",
    "finishDate": "2019-01-03",
    "startHour": "05",
    "finishHour": "23",
    "unionUnit": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "name": "Nome da unidade sindical"
    }
  }]
}
```

### POST /agnu

Cria uma AGNU.

#### JSON de entrada

```json
{
  "name": "Nome da AGNU",
  "startDate": "2019-01-01",
  "finishDate": "2019-01-03",
  "startHour": "05",
  "finishHour": "23",
  "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35"
}
```

### PUT /agnu

Atualiza os dados da AGNU.

#### JSON de entrada

```json
{
  "id": "65083027-c974-498d-974b-9ee04baf7b58",
  "name": "Nome da AGNU",
  "startDate": "2019-01-01",
  "finishDate": "2019-01-03",
  "startHour": "05",
  "finishHour": "23",
  "unionUnitID": "65083027-c978-498d-974b-9ee04baf7b35"
}
```

- `id`: ID da AGNU
- `nome`: Nome da AGNU
- `startDate`: Data de início da AGNU
- `finishDate`: Data de término da AGNU
- `startHour`: Hora de início da AGNU
- `finishHour`: Hora de término da AGNU
- `unionUnitID`: ID da unidade sindical res´ponsável pela AGNU

### GET /agnu/{id}/questions

Retorna a lista de perguntas da AGNU.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "question": "Texto da pergunta",
    "order": 1,
    "type": 1,
    "options": [{
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "order": 1,
      "option": "Texto da resposta 1"
    },
    {
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "order": 2,
      "option": "Texto da resposta 2"
    }]
  }]
}
```

- `mensagem`: Texto a ser exibido ao usuário
- `dados`: Dados da pergunta
- `tipo`: Tipo da pergunta. 1 para escolha única, 2 para múltipla escolha

### GET /agnu/questions/{id}

Retorna os dados de uma pergunta da AGNU.

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "text": "Texto da pergunta",
    "order": 1,
    "type": 1
  }
}
```

- `id`: ID da pergunta
- `text`: Texto da pergunta
- `order`: Ordem da pergunta
- `type`: Tipo da pergunta. 1 para escolha única, 2 para múltipla escolha

### POST /agnu/questions

Cria uma pergunta para a AGNU.

#### JSON de entrada

```json
{
  "agnuID": "65083027-c974-498d-974b-9ee04baf7b58",
  "text": "Texto da pergunta",
  "order": 1,
  "type": 1,
}
```

### PUT /agnu/questions

Atualiza os dados de uma pergunta da AGNU.

#### JSON de entrada

```json
{
  "id": "65083027-c974-498d-974b-9ee04baf7b58",
  "text": "Texto da pergunta",
  "order": 1,
  "type": 1,
}
```

- `id`: ID da pergunta
- `text`: Texto da pergunta
- `order`: Ordem da pergunta
- `type`: Tipo da pergunta. 1 para escolha única, 2 para múltipla escolha

### DELETE /agnu/questions/{id}

Remove uma pergunta da AGNU.


### GET /agnu/questions/{id}/options

Retorna a lista de opções de uma pergunta da AGNU.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "option": "Texto da resposta 1",
    "order": 1,
    "indicated": true
  },
  {
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "option": "Texto da resposta 2",
    "order": 2,
    "indicated": false
  }]
}
```

- `id`: ID da opção
- `option`: Texto da opção
- `order`: Ordem da opção
- `indicated`: Indica se a opção foi indicada como resposta

### GET /agnu/questions/options/{id}

Retorna os dados de uma opção de uma pergunta da AGNU.

#### JSON de retorno

```json
{
  "data": {
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "option": "Texto da resposta 1",
    "order": 1,
    "indicated": true
  }
}
```

### POST /agnu/questions/options

Cria uma opção para uma pergunta da AGNU.

#### JSON de entrada

```json
{
  "questionID": "65083027-c974-498d-974b-9ee04baf7b58",
  "option": "Texto da resposta 1",
  "order": 1,
  "indicated": true
}
```

- `option`: Texto da opção
- `indicated`: Indica se a opção foi indicada como resposta

### PUT /agnu/questions/options

Atualiza os dados de uma opção de uma pergunta da AGNU.

#### JSON de entrada

```json
{
  "id": "65083027-c974-498d-974b-9ee04baf7b58",
  "option": "Texto da resposta 1",
  "order": 1,
  "indicated": true
}
```

### DELETE /agnu/options/{id}

Remove uma opção de uma pergunta da AGNU.


### POST /answer

Responde uma pergunta da AGNU.

#### JSON de entrada

```json
{
  "question": "65083027-c974-498d-974b-9ee04baf7b58",
  "answers": ["65083027-c974-498d-974b-9ee04baf7b58"]
}
```

- `pergunta`: ID da pergunta
- `respostas`: ID das respostas

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```

- `mensagem`: Texto a ser exibido ao usuário

### GET /agnu/{id}/answer

Retorna a lista de respostas do usuário.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "question": {
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "question": "Texto da pergunta",
      "type": 1
    },
    "answers": [{
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "resposta": "Texto da resposta"
    }]
  }]
}
```

- `mensagem`: Texto a ser exibido ao usuário
- `dados`: Dados da resposta

### PUT /agnu/{id}/confirm

Confirma a votação do usuário.

#### JSON de retorno

```json
{
  "data": {
    "validacao": "código de validação"
  },
  "message": "texto a ser exibido ao usuário"
}
```

### GET /agnu/{id}/details

Retorna o resumo da AGNU para ser exibido como detalhes da AGNU

#### JSON de Retorno

```json
  {
    "data": {
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "name": "Nome da AGNU",
      "start": "2019-01-01 12:34:45",
      "end": "2019-01-03 12:34:45",
      "unionUnit": {
        "id": "65083027-c978-498d-974b-9ee04baf7b35",
        "name": "Nome da unidade sindical"
      },
      "votes": [{
        "label": "Ativos",
        "value": 903
      },
      {
        "label": "Aposentados",
        "value": 380
      }],
      "left": [{
        "label": "Ativos",
        "value": 9785
      },
      {
        "label": "Aposentados",
        "value": 2980
      }]
    }
  }
```

### GET /agnu/{id}/unionUnits/{id}/details

Retorna o resumo da AGNU para ser exibido como detalhes da AGNU

#### JSON de Retorno

```json
  {
    "data": {
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "name": "Delegacia Sindical de Teste",
      "votes": [{
        "label": "Ativos",
        "value": 90
      },
      {
        "label": "Aposentados",
        "value": 38
      }],
      "left": [{
        "label": "Ativos",
        "value": 978
      },
      {
        "label": "Aposentados",
        "value": 298
      }]
    }
  }
```

### GET /agnu/{id}/voters

Retorna a lista de votantes da AGNU.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "name": "Nome do votante",
    "unionUnit": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "name": "Nome da unidade sindical"
    },
    "funtionalStatus": "Ativo",
    "validationCode": "c26581cd0f38a821802a903b23f4ce6a16c56361d4900d50bb3ccbc0a8d6c63a",
    "dateHour": "2019-01-01 12:34:45"
  }]
}
```
