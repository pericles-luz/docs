# docs
Documentações


## Lista de API

- [AGNU](agnu/README.md)
- [Ressarcimento](ressarcimento/README.md)
- [Viagem](viagem/README.md)
- [Rateio](rateio/README.md)


## Rotas de autenticação a serem utilizadas para todas as APIs

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
  "message": "texto a ser exibido ao usuário",
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
  "data": {
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
  - 2: Filiado - pode acessar áreas restritas a filiados
  - 4: Servidor - pode acessar áreas restritas a servidores
  - 8: Aprovador - pode aprovar requisições
  - 16: Pagador - pode informar que o pagamento foi realizado
  - 32: Dirigente sindical - pode acessar áreas restritas a dirigentes sindicais
  - 64: Financeiro - pode acessar áreas restritas ao financeiro
  - 128: Comprador - pode informar a compra de passagens e reservas em hotéis


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

## Rotas de uso geral entre os módulos

### GET /airports/{state}

Retorna a lista de aeroportos do estado informado. Deve ser informada a sigla do estado.

#### JSON de retorno

```json
[
  {
    "id": 1,
    "name": "Nome do aeroporto",
    "iata": "ABC",
    "city": "Nome da cidade",
    "state": "Sigla do estado"
  }
]
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

### GET /costCenters/{unionUnitID}

Retorna os centros de custo cadastrados para o unidade sindical.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "description": "Centro de custo 1",
      "classification": "Classificação do centro de custo",
      "unionUnitID": "uuidv4",
      "type": 1
    },
    {
      "id": "uuidv4",
      "description": "Centro de custo 1",
      "classification": "Classificação do centro de custo",
      "unionUnitID": "uuidv4",
      "type": 2
    }
  ]
}
```

### POST /costCenters

Grava um centro de custo.

#### JSON de entrada
  
  ```json
  {
    "description": "Centro de custo 1",
    "classification": "Classificação do centro de custo",
    "unionUnitID": "uuidv4",
    "type": 2
  }
  ```
  
### PUT /costCenters

Grava um centro de custo.

#### JSON de entrada
  
  ```json
  {
    "id": "uuidv4",
    "description": "Centro de custo 1",
    "classification": "Classificação do centro de custo",
    "unionUnitID": "uuidv4",
    "type": 2
  }
  ```

- `id` (opcional): ID do centro de custo
- `description` (obrigatório): Descrição do centro de custo
- `classification` (opcional): Classificação do centro de custo no sistema contábil
- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`)
- `type` (obrigatório): Tipo do centro de custo. 1 normal, 2 artigo 132

### GET /unionUnits

Retorna as unidades pagadoras cadastradas.


### GET /dailyTypes/{unionUnitID}

Retorna os valores de diárias cadastradas para o usuário.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "type": 1,
      "description": "Diária parcial",
      "legalBasis": "Artigo 32 do Estatuto",
      "value": 10000
    },
    {
      "id": "uuidv4",
      "type": 2,
      "description": "Diária cheia",
      "legalBasis": "Ata da Assembleia de 23-08-220",
      "value": 20000
    }
  ]
}
```

### POST /dailyTypes

Registra um valor de diária.

#### JSON de entrada
  
  ```json
  {
    "unionUnitID": "uuidv4",
    "type": 1,
    "description": "Diária parcial",
    "legalBasis": "Artigo 32 do Estatuto",
    "value": 10000
  }
  ```

- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`). Use `99999999-9999-4999-9999-999999999999` para registrar um valor de diária para todas as unidades sindicais
- `type` (obrigatório): Tipo da diária. 1 cheia, 2 parcial, 3 urbana, 4 Ajuda de Custo para Reunião Virtual ou Remota
- `description` (obrigatório): Descrição da diária
- `value` (obrigatório): Valor da diária
- `legalBasis` (opcional): Base legal da diária

### PUT /dailyTypes

Altera um valor de diária.

#### JSON de entrada
  
  ```json
  {
    "id": "uuidv4",
    "type": 1,
    "description": "Diária parcial",
    "legalBasis": "Artigo 32 do Estatuto",
    "value": 10000
  }
  ```

- `id` (obrigatório): ID do valor de diária
- `type` (obrigatório): Tipo da diária. 1 cheia, 2 parcial, 3 urbana, 4 Ajuda de Custo para Reunião Virtual ou Remota
- `description` (obrigatório): Descrição da diária
- `value` (obrigatório): Valor da diária
- `legalBasis` (opcional): Base legal da diária
- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`). Use `99999999-9999-4999-9999-999999999999` para registrar um valor de diária para todas as unidades sindicais

### DELETE /dailyTypes/{id}

Exclui um valor de diária.

### GET /movingBudgets/{unionUnitID}

Retorna os valores de auxílio deslocamento cadastrados para a unidade sindical.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "description": "Auxílio deslocamento",
      "legalBasis": "Artigo 32 do Estatuto",
      "value": 10000
    }
  ]
}
```

- `id` (obrigatório): ID do valor de auxílio deslocamento
- `description` (obrigatório): Descrição do auxílio deslocamento
- `value` (obrigatório): Valor do auxílio deslocamento
- `legalBasis` (opcional): Base legal do auxílio deslocamento

### POST /movingBudgets

Registra um valor de auxílio deslocamento.

#### JSON de entrada
  
  ```json
  {
    "unionUnitID": "uuidv4",
    "description": "Auxílio deslocamento",
    "legalBasis": "Artigo 32 do Estatuto",
    "value": 10000
  }
  ```

- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`). Use `99999999-9999-4999-9999-999999999999` para registrar um valor de auxílio deslocamento para todas as unidades sindicais
- `description` (obrigatório): Descrição do auxílio deslocamento
- `value` (obrigatório): Valor do auxílio deslocamento
- `legalBasis` (opcional): Base legal do auxílio deslocamento

### PUT /movingBudgets

Altera um valor de auxílio deslocamento.

#### JSON de entrada
  
  ```json
  {
    "id": "uuidv4",
    "description": "Auxílio deslocamento",
    "legalBasis": "Artigo 32 do Estatuto",
    "value": 10000
  }
  ```

- `id` (obrigatório): ID do valor de auxílio deslocamento
- `description` (obrigatório): Descrição do auxílio deslocamento
- `value` (obrigatório): Valor do auxílio deslocamento
- `legalBasis` (opcional): Base legal do auxílio deslocamento
- `unionUnitID` (obrigatório): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`). Use `99999999-9999-4999-9999-999999999999` para registrar um valor de auxílio deslocamento para todas as unidades sindicais

### DELETE /movingBudgets/{id}

Exclui um valor de auxílio deslocamento.