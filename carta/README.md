# API para solicitação e gerenciamento de Carta-Fiança

## Objetivo

Este documento tem como objetivo descrever a API que permite a solicitação e gerenciamento de Carta-Fiança.

## Visão Geral

O fluxo se inicia com a solicitação feita pelo filiado. Depois é analisada pela secretaria, o parecer jurídico é anexado e, por fim, a diretoria aprova ou não a solicitação. 

Caso a solicitação seja aprovada, a carta é emitida, assinda digitalmente pelo presidente e pelo diretor financeiro e, então, anexada ao pedido.

## Solicitação

### Solicitar Carta-Fiança

### POST /letterOfGuarantee/requests

Registra uma solicitação de Carta-Fiança.

#### JSON de entrada

```json
{
    "rentalValue": 200000,
    "start": "2020-01-01",
    "finish": "2020-12-31",
    "accountID": "uuidv4",
    "hasProperties": false
}
```

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "start": "2020-01-01",
        "finish": "2020-12-31",
        "accountID": "uuidv4",
        "hasProperties": false,
        "status": 2,
        "number": "23",
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### PUT /letterOfGuarantee/requests/{id}

Altera uma solicitação de Carta-Fiança.

#### JSON de entrada

```json
{
    "rentalValue": 200000,
    "start": "2020-01-01",
    "finish": "2020-12-31",
    "accountID": "uuidv4",
    "hasProperties": false
}
```

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "start": "2020-01-01",
        "finish": "2020-12-31",
        "accountID": "uuidv4",
        "hasProperties": false,
        "status": 2,
        "number": "23",
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### GET /letterOfGuarantee/requests/{id}

Retorna uma solicitação de Carta-Fiança.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "start": "2020-01-01",
        "finish": "2020-12-31",
        "accountID": "uuidv4",
        "hasProperties": false,
        "status": 2,
        "number": "23",
        "requester": {
            "id": "uuidv4",
            "name": "João da Silva"
        }
    }
}
```

### GET /letterOfGuarantee/requests

Retorna uma lista de solicitações de Carta-Fiança.

#### JSON de retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "rentalValue": 200000,
            "start": "2020-01-01",
            "finish": "2020-12-31",
            "accountID": "uuidv4",
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
            "start": "2020-01-01",
            "finish": "2020-12-31",
            "accountID": "uuidv4",
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

### PUT /letterOfGuarantee/requests/cancel

Cancela uma solicitação de Carta-Fiança. O botão de cancelar só pode aparecer para o autor da solicitação e não deve aparecer se estiver nos status DENIED(5) ou CANCELED(6).

No modal com o formulário para cancelamento, deve ser exibido um campo para o usuário informar o motivo do cancelamento.
Também é importante destacar que o campo de motivo deve ser obrigatório e o seu label deve ser "Informe o motivo do cancelamento".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Motivo do cancelamento"
}
```

### PUT /letterOfGuarantee/requests/approve

Aprova uma solicitação de Carta-Fiança. O botão de aprovação só deve aparecer para a secretária(permissão 1024) e só pode aparecer se a solicitação estiver no status LEGALY_APPROVED(11).

No modal com o formulário para aprovação, deve ser exibido um campo para o usuário fazer alguma observação.
Também é importante destacar que o campo de observação deve ser opcional e o seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação",
    "value": 200000,
    "start": "2020-01-01",
    "finish": "2020-12-31",
}
```

### PUT /letterOfGuarantee/requests/deny

Reprova uma solicitação de Carta-Fiança. O botão de negar só deve aparecer para a secretária(permissão 1024) e só pode aparecer se a solicitação estiver no status LEGALY_APPROVED(11) ou SUBMITTED(2).

No modal com o formulário para reprovação, deve ser exibido um campo para o usuário informar o motivo da reprovação.
Também é importante destacar que o campo de motivo deve ser obrigatório e o seu label deve ser "Informe o motivo da negativa".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Motivo da reprovação"
}
```

### PUT /letterOfGuarantee/requests/sendToLegal

Envia uma solicitação de Carta-Fiança para o parecer jurídico. O botão de enviar para o parecer jurídico só deve aparecer para a secretária(permissão 1024) e só pode aparecer se a solicitação estiver no status SUBMITTED(2).

No modal com o formulário para envio para o parecer jurídico, deve ser exibido um campo para o usuário fazer alguma observação.
Também é importante destacar que o campo de observação deve ser opcional e o seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação"
}
```

### PUT /letterOfGuarantee/requests/legalOpinion

Inclui o parecer jurídico a uma solicitação de Carta-Fiança. O botão de incluir parecer jurídico só deve aparecer para o advogado(permissão 2048) e só pode aparecer se a solicitação estiver no status SENT_TO_LEGAL_OPINION(10).

No modal com o formulário para incluir o parecer jurídico, deve ser exibido um campo para o usuário fazer alguma observação.
Também é importante destacar que o campo de observação deve ser opcional, se aprovado e, caso não seja aprovado, o campo é obrigatório. O seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação",
    "legalyApproved": true
}
```

## Rotas para uploads de arquivos


### POST /letterOfGuarantee/requests/{id}/attachments/{type}

Adiciona um anexo a uma solicitação de carta-fiança. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64, podendo ser PDF, PNG ou JPG.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `5`: Contrato de locação
  - `6`: Carta-fiança assinada

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

### GET /letterOfGuarantee/{id}/attachments

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
