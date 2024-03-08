# docs
Documentações


## Lista de API

- [AGNU](agnu/README.md)
- [Ressarcimento](ressarcimento/README.md)
- [Viagem](viagem/README.md)
- [Rateio](rateio/README.md)
- [Carta-fiança](carta/README.md)
- [Prestação de Contas](prestacao/README.md)
- [AJI](aji/README.md)
- [Filiação](filiacao/README.md)
- [Demonstrativo IRPF](demonstrativo/README.md)
- [Agendamento](agendamento/README.md)
- [Votação](votacao/README.md)
- [Propostas de teses](proposta/README.md)


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

### GET /persons/{id}/authorizations

Retorna as permissões que foram autorizadas para o usuário.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "authorizedID": "uuidv4",
      "authorizedName": "Nome da permissão",
      "authorizerID": "uuidv4",
      "authorizerName": "Nome do autorizador",
      "permission": 8,
      "description": "Descrição da permissão"
    },
    {
      "id": "uuidv4",
      "authorizedID": "uuidv4",
      "authorizedName": "Nome da permissão",
      "authorizerID": "uuidv4",
      "authorizerName": "Nome do autorizador",
      "unionUnitID": "uuidv4",
      "unionUnitName": "Nome da unidade sindical",
      "permission": 8,
      "description": "Descrição da permissão"
    }
  ]
}
```

### POST /authorizations

Registra uma permissão para o usuário.

#### JSON de entrada
  
  ```json
  {
    "authorizedID": "uuidv4",
    "unionUnitID": "uuidv4",
    "permission": 8
  }
  ```

### PUT /authorizations

Altera uma permissão para o usuário.

#### JSON de entrada
  
  ```json
  {
    "id": "uuidv4",
    "authorizedID": "uuidv4",
    "unionUnitID": "uuidv4",
    "permission": 8
  }
  ```

### DELETE /authorizations/{id}

Exclui uma permissão para o usuário.

- `id` (opcional para POST): ID da permissão
- `authorizedID` (obrigatório): ID da pessoa a ser autorizada. (buscar pessoas em `/persons/{cpfOrName}`)
- `unionUnitID` (obrigatório): ID da unidade sindical (buscar unidades sindicais em `/unionUnits`)
- `permission` (obrigatório): Permissão a ser autorizada. (buscar permissões em `/permissions/delegables`)

### GET /permissions/delegables

Retorna as permissões que podem ser delegadas pelo usuário.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": 1,
      "description": "Descrição da permissão"
    },
    {
      "id": 2,
      "description": "Descrição da permissão"
    }
  ]
}
```

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
      "name": "Centro de custo 1",
      "classification": "Classificação do centro de custo",
      "unionUnitID": "uuidv4",
      "isPublic": true,
      "isArticle132": false
    },
    {
      "id": "uuidv4",
      "name": "Centro de custo 1",
      "classification": "Classificação do centro de custo",
      "unionUnitID": "uuidv4",
      "isPublic": true,
      "isArticle132": false
    }
  ]
}
```

### POST /costCenters

Grava um centro de custo.

#### JSON de entrada
  
  ```json
  {
    "name": "Centro de custo 1",
    "classification": "Classificação do centro de custo",
    "unionUnitID": "uuidv4",
    "isPublic": true,
    "isArticle132": false
  }
  ```
  
### PUT /costCenters

Grava um centro de custo.

#### JSON de entrada
  
  ```json
  {
    "id": "uuidv4",
    "name": "Centro de custo 1",
    "classification": "Classificação do centro de custo",
    "unionUnitID": "uuidv4",
    "isPublic": true,
    "isArticle132": false
  }
  ```

- `id` (obrigatório no PUT): ID do centro de custo
- `name` (obrigatório): Nome do centro de custo
- `classification` (opcional): Classificação do centro de custo no sistema contábil
- `unionUnitID` (opcional): ID da unidade pagadora (buscar unidades pagadoras em `/unionUnits`)
- `isPublic` (obrigatório): Indica se o centro de custo é público
- `isArticle132` (obrigatório): Indica se o centro de custo é do artigo 132

### GET /costCenters/{id}/persons

Lista as pessoas associadas a um centro de custo

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "name": "Joaquim da Silva"
    },
    {
      "id": "uuidv4",
      "name": "Joana Cruz"
    }
  ]
}
```

### POST /costCenters/{id}/addPerson

Adiciona uma pessoa a um centro de custo

#### parâmetros

```json
{
  "personID": "uuidv4"
}
```

Estando tudo certo, retorna 204.

### DELETE /costCenters/{id}/persons/{personID}

Retira uma pessoa de um centro de custo.

Estando tudo certo, retorna 204.

### GET /persons/{cpfOrName}

Retorna as pessoas que atendam ao filtro.

#### JSON de retorno

```json
{
  "data": [
    {
      "id": "uuidv4",
      "name": "Nome da pessoa",
      "cpf": "00000000191"
    },
    {
      "id": "uuidv4",
      "name": "Nome da pessoa",
      "cpf": "00000000191"
    }
  ]
}
```

- `cpfOrName` (obrigatório): parte do CPF ou nome da pessoa a ser buscada

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
- `type` (obrigatório): Tipo da diária. 
  - 1 cheia
  - 2 parcial
  - 3 urbana
  - 4 Ajuda de Custo para Reunião Virtual ou Remota
  - 5 Diárias de eventos
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
- `type` (obrigatório): Tipo da diária. 
  - 1 cheia
  - 2 parcial
  - 3 urbana
  - 4 Ajuda de Custo para Reunião Virtual ou Remota
  - 5 Diárias de eventos
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


### GET /accounts

Retorna as contas correntes cadastradas para o usuário. Retorna também as chaves pix cadastradas para o usuário.
O campo `kind` indica se é uma conta corrente ou uma chave pix. 1 para conta corrente e 2 para chave pix.

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
			"type": 1,
      "kind": 1
		},
    {
      "key": "12345678901",
      "id": "uuidv4",
      "kind": 2
    }]
}
```

### GET /unionUnits/{unionUnitID}/accounts

Retorna as contas correntes cadastradas para a unidade sindical passada como parâmetro.

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
			"type": 1,
      "kind": 1
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
			"type": 1,
      "kind": 1
		}
}
```

### POST /unionUnits/{unionUnitID}/accounts

Grava uma conta corrente de unidade sindical.

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
			"type": 1,
      "kind": 1
		}
}
```

### POST /pix

Grava uma chave pix.

#### Parâmetros

```json
{
  "data": {
      "key": "12345678901",
    }
}
```

#### JSON de retorno

```json
{
  "data": {
      "key": "12345678901",
      "id": "uuidv4",
      "kind": 2
    }
}
```

### GET /pix/:id

Retorna uma chave pix.

#### JSON de retorno

```json
{
  "data": {
      "key": "12345678901",
      "id": "uuidv4",
      "kind": 2
    }
}
```

### DELETE /pix/:id

Exclui uma chave pix.

Retorna 204 em caso de sucesso.