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

- `title` campo texto com no máximo 250 caracteres;
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

### POST /aji/requests

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
        "requester": {
            "id": "uuidv4",
            "name": "string",
        },
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### GET /aji/requests

Retorna uma lista de pedidos de AJI.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "brief": "string",
            "specialNeeds": "string",
            "requester": {
                "id": "uuidv4",
                "name": "string",
            },
            "lawyer": {
                "id": "uuidv4",
                "name": "string",
            },
            "approver": {
                "id": "uuidv4",
                "name": "string",
            },
            "ruleRelated": true,
            "note": "string",
            "statusID": 1
        },
        {
            "id": "uuidv4",
            "brief": "string",
            "specialNeeds": "string",
                        "requester": {
                "id": "uuidv4",
                "name": "string",
            },
            "lawyer": {
                "id": "uuidv4",
                "name": "string",
            },
            "approver": {
                "id": "uuidv4",
                "name": "string",
            },
            "ruleRelated": true,
            "note": "string",
            "statusID": 1
        }
    ]
}
```

### GET /aji/requests/{id}

Retorna um pedido de AJI.

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "brief": "string",
        "specialNeeds": "string",
        "requester": {
            "id": "uuidv4",
            "name": "string",
        },
        "lawyer": {
            "id": "uuidv4",
            "name": "string",
        },
        "approver": {
            "id": "uuidv4",
            "name": "string",
        },
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### PUT /aji/requests/{id}

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
        "requester": {
            "id": "uuidv4",
            "name": "string",
        },
        "lawyer": {
            "id": "uuidv4",
            "name": "string",
        },
        "approver": {
            "id": "uuidv4",
            "name": "string",
        },
        "ruleRelated": true,
        "note": "string",
        "statusID": 1
    }
}
```

### PUT /aji/requests/{id}/submit

Encaminha um pedido de AJI.

#### Parâmetros

```json
{
    "note": "string"
}
```

- `note` texto com no máximo 250 caracteres, não obrigatório;

#### Retorno

Não retorna dados. Retorna status 204.

### PUT /aji/requests/{id}/deferral

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

### PUT /aji/requests/{id}/cancel

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


### PUT /aji/requests/{id}/requestDocument

Solicita um documento para um pedido de AJI.
O status da AJI mudará para `solicitado documento(23)`.

#### Parâmetros

```json
{
    "note": "string"
}
```


### POST /aji/requests/{id}/attachments/{type}

Adiciona um anexo a uma solicitação de AJI. Importante observar que o anexo deve ser enviado em base64, num JSON onde conste também o título dado ao anexo.
O arquivo pode ser PDF, PNG ou JPG.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `7`: Documento a ser anexado ao pedido de AJI;

#### payload de entrada

```
{
  "title": "nome do arquivo",
  "file": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAAEnQAABJ0Ad5mH3gAAAAMSURBVBhXY3growIAAycBLhVrvukAAAAASUVORK5CYII="
}
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
