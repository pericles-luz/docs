# API para solicitação e gerenciamento de Carta-Fiança

## Objetivo

Este documento tem como objetivo descrever a API que permite a solicitação e gerenciamento de Carta-Fiança.

## Visão Geral

O fluxo se inicia com a solicitação feita pelo filiado. Depois é analisada pela secretaria, o parecer jurídico é anexado e, por fim, a diretoria aprova ou não a solicitação. 

Caso a solicitação seja aprovada, a carta é emitida, assinda digitalmente pelo presidente e pelo diretor financeiro e, então, anexada ao pedido.

## Solicitação

### Solicitar Carta-Fiança

### POST /letter-of-guarantee/requests

Registra uma solicitação de Carta-Fiança.

#### JSON de entrada

```json
{
    "rentalValue": 200000,
    "startDate": "2020-01-01",
    "endDate": "2020-12-31",
    "hasProperties": false
}
```

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "startDate": "2020-01-01",
        "endDate": "2020-12-31",
        "hasProperties": false,
        "status": 2,
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### PUT /letter-of-guarantee/requests/{id}

Altera uma solicitação de Carta-Fiança.

#### JSON de entrada

```json
{
    "rentalValue": 200000,
    "startDate": "2020-01-01",
    "endDate": "2020-12-31",
    "hasProperties": false
}
```

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "startDate": "2020-01-01",
        "endDate": "2020-12-31",
        "hasProperties": false,
        "status": 2,
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### GET /letter-of-guarantee/requests/{id}

Retorna uma solicitação de Carta-Fiança.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "startDate": "2020-01-01",
        "endDate": "2020-12-31",
        "hasProperties": false,
        "status": 2,
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### GET /letter-of-guarantee/requests

Retorna uma lista de solicitações de Carta-Fiança.

#### JSON de retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "rentalValue": 200000,
            "startDate": "2020-01-01",
            "endDate": "2020-12-31",
            "hasProperties": false,
            "status": 2,
            "requester": {
                "id": "uuidv4",
                "name": "João da Silva"
            }
        },
        {
            "id": "uuidv4",
            "rentalValue": 200000,
            "startDate": "2020-01-01",
            "endDate": "2020-12-31",
            "hasProperties": false,
            "status": 2,
            "requester": {
                "id": "uuidv4",
                "name": "João da Silva"
            }
        }
    ]
}
```

### PUT /letter-of-guarantee/requests/{id}/cancel

Cancela uma solicitação de Carta-Fiança.

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Motivo do cancelamento"
}
```

### PUT /letter-of-guarantee/requests/{id}/approve

Aprova uma solicitação de Carta-Fiança.

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação",
    "value": 200000,
    "startDate": "2020-01-01",
    "endDate": "2020-12-31",
}
```

### PUT /letter-of-guarantee/requests/{id}/deny

Reprova uma solicitação de Carta-Fiança.

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Motivo da reprovação"
}
```

### PUT /letter-of-guarantee/requests/{id}/send-to-legal

Envia uma solicitação de Carta-Fiança para o parecer jurídico.

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação"
}
```

### PUT /letter-of-guarantee/requests/{id}/legal-opinion

Inclui o parecer jurídico a uma solicitação de Carta-Fiança.

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação",
    "legalOpinion": "Parecer jurídico"
}
```

## Rotas para uploads de arquivos


### POST /letter-of-guarantee/{id}/attachments/{type}

Adiciona um anexo a uma solicitação de carta-fiança. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64, podendo ser PDF, PNG ou JPG.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `5`: Contrato de locação

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
  "message": "texto a ser exibido ao usuário"
}
```

### GET /letter-of-guarantee/{id}/attachments

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
  "message": "texto a ser exibido ao usuário"
}
```
