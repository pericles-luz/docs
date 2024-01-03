# API para Assistência Jurídica Individual

## Objetivo

O objetivo deste projeto é criar o sistema de Assistência Jurídica Individual (AJI) do Sindireceita.

O filiado poderá solicitar a AJI através do sistema, que será encaminhado para a Diretoria de Assuntos Jurídicos (DAJ) do Sindireceita, que irá analisar e deferir ou não a solicitação.

## Formulário de solicitação

O formulário de solicitação de AJI será preenchido pelo filiado e conterá as seguintes informações:

- Resumo do pedido `brief`, deve ser um campo texto com no máximo 5000 caracteres;
- Caso seja portador de necessidade especial, informar no campo `specialNeeds` em texto.

Depois de preenchido, o pedido deverá mostrar as seguintes opções ao solicitante:

- `Encaminhar...` para abrir o modal de encaminhamento;
- `Editar...` para editar o pedido;
- `Anexar documento...` para anexar documentos ao pedido;
- `Cancelar...` para cancelar o pedido.

Os campos `functionRelated`, `lawyerID` e `note` só devem ser mostrados se o status for diferente de `encaminhada(2)`.

### Modal de encaminhamento

O modal de encaminhamento deverá conter os seguintes campos:

- `note` campo texto com no máximo 250 caracteres;

### Modal de anexar documento

O modal de anexar documento deverá conter os seguintes campos:

- `name` campo texto com no máximo 250 caracteres;
- `file` campo para upload de arquivo.

### Modal de cancelar pedido

O modal de cancelar pedido deverá conter os seguintes campos:

- `note` campo texto com no máximo 250 caracteres;

### Modal de deferir pedido

O modal de deferir pedido deverá conter os seguintes campos:

- `deferred` campo booleano, indica se o pedido foi deferido ou não;
- `ruleRelated` campo booleano, indica se o pedido está relacionado à função do filiado no trabalho;
- `note` campo texto com no máximo 250 caracteres;

O botão `Analisar...` só deve ser mostrado se o status for `encaminhada(2)` e o usuário for advogado(permissão 2048).

## Endpoints da API

### POST /api/aji

Cria um novo pedido de AJI.

#### Parâmetros

```json
{
  "brief": "string",
  "specialNeeds": "string"
}
```

- `brief` texto com no máximo 5000 caracteres;
- `specialNeeds` texto com no máximo 250 caracteres.


#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "brief": "string",
        "specialNeeds": "string",
        "lawyerID": "uuidv4",
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### GET /api/aji

Retorna uma lista de pedidos de AJI.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "brief": "string",
            "specialNeeds": "string",
            "lawyerID": "uuidv4",
            "ruleRelated": true,
            "note": "string",
            "statusID": 1
        },
        {
            "id": "uuidv4",
            "brief": "string",
            "specialNeeds": "string",
            "lawyerID": "uuidv4",
            "ruleRelated": true,
            "note": "string",
            "statusID": 1
        }
    ]
}
```

### GET /api/aji/{id}

Retorna um pedido de AJI.

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "brief": "string",
        "specialNeeds": "string",
        "lawyerID": "uuidv4",
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### PUT /api/aji/{id}

Atualiza um pedido de AJI.

#### Parâmetros

```json
{
  "brief": "string",
  "specialNeeds": "string"
}
```

- `brief` texto com no máximo 5000 caracteres;
- `specialNeeds` texto com no máximo 250 caracteres.

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "brief": "string",
        "specialNeeds": "string",
        "lawyerID": "uuidv4",
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### PUT /api/aji/{id}/deferral

Deferir um pedido de AJI.

#### Parâmetros

```json
{
    "deferred": true,
    "ruleRelated": true,
    "note": "string",
    "lawyerID": "uuidv4"
}
```

- `deferred` indica se o pedido foi deferido ou não;
- `ruleRelated` indica se o pedido está relacionado à função do filiado no trabalho;

#### Retorno

Não retorna dados. Retorna status 204.

### PUT /api/aji/{id}/cancel

Cancela um pedido de AJI.

#### Parâmetros

```json
{
    "note": "string"
}
```

- `note` texto com no máximo 250 caracteres;

#### Retorno

Não retorna dados. Retorna status 204.

