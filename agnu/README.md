# API para AGNU


## Rotas de autenticação

### POST /autenticar/token

Envia um token de acesso para o usuário. O token é enviado por SMS ou e-mail, dependendo do valor de `meioEnvio`.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "sentMedia": 1,
}
```

- `cpf`: CPF do usuário
- `meioEnvio`: Meio de envio do token de acesso. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
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
  "message": "texto a ser exibido ao usuário"
}
```

- `bearerToken`: Token de acesso para as demais rotas
- `mensagem`: Texto a ser exibido ao usuário

O token de acesso é válido por 24 horas e seu payload será:

```json
{
  "exp": 1234567890,
  "iss": "sindireceita.org.br",
  "data": {
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

### GET /agnu/opened

Retorna a lista de AGNU abertas.

#### JSON de retorno

```json

{
  "message": "texto a ser exibido ao usuário",
  "data":   [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "nome": "Nome da AGNU",
    "inicio": "2019-01-01 12:34:45",
    "fim": "2019-01-03 12:34:45",
    "unidadeSindical": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "nome": "Nome da unidade sindical"
    }
  }]
}

```

- `mensagem`: Texto a ser exibido ao usuário
- `dados`: Dados básicos da AGNU

### GET /agnu/{id}/answers

Retorna a lista de perguntas da AGNU.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
  "data": [{
    "id": "65083027-c974-498d-974b-9ee04baf7b58",
    "question": "Texto da pergunta",
    "type": 1,
    "options": [{
      "id": "65083027-c974-498d-974b-9ee04baf7b58",
      "resposta": "Texto da resposta"
    }]
  }]
}
```

- `mensagem`: Texto a ser exibido ao usuário
- `dados`: Dados da pergunta
- `tipo`: Tipo da pergunta. 1 para escolha única, 2 para múltipla escolha

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