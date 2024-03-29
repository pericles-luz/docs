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
    "lessor": "João da Silva",
    "administrator": "Joaquim da Silva",
    "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
    "letterValue": 200000,
    "authorizeDiscount": false
}
```
Se `authorizeDiscount` for `false`, não deve liberar o botão de finalizar solicitação.
Os campos `lessor`, `administrator`, `letterValue` e `address` só devem ser editados pela secretária(permissão 1024).

O campo `authorizeDiscount` deve ter o label `Autorizar desconto em folha?` e, no hint da interrogação, deve ter o texto `Autorizo o desconto em folha do total de despesas aportadas pelo Sindireceita, resultado da inadimplência relativa a esta Carta de Fiança Locatícia.`

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "start": "2020-01-01",
        "finish": "2020-12-31",
        "accountID": "uuidv4",
        "authorizeDiscount": true,
        "lessor": "João da Silva",
        "administrator": "Joaquim da Silva",
        "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
        "letterValue": 200000,
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
    "lessor": "João da Silva",
    "administrator": "Joaquim da Silva",
    "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
    "letterValue": 200000,
    "authorizeDiscount": true
}
```

Os campos `lessor`, `administrator`, `letterValue` e `address` só devem ser editados pela secretária(permissão 1024).

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "rentalValue": 200000,
        "start": "2020-01-01",
        "finish": "2020-12-31",
        "accountID": "uuidv4",
        "authorizeDiscount": true,
        "lessor": "João da Silva",
        "administrator": "Joaquim da Silva",
        "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
        "letterValue": 200000,
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
        "authorizeDiscount": true,
        "lessor": "João da Silva",
        "administrator": "Joaquim da Silva",
        "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
        "letterValue": 200000,
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
            "authorizeDiscount": true,
            "status": 2,
            "lessor": "João da Silva",
            "administrator": "Joaquim da Silva",
            "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
            "letterValue": 200000,
            "number": 23,
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
            "authorizeDiscount": true,
            "lessor": "João da Silva",
            "administrator": "Joaquim da Silva",
            "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
            "letterValue": 200000,
            "status": 2,
            "number": 23,
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

Aprova uma solicitação de Carta-Fiança. O botão de aprovação só deve aparecer para a secretária(permissão 1024) e só pode aparecer se a solicitação estiver no status CONSENTED(26).

No modal com o formulário para aprovação, deve ser exibido um campo para o usuário fazer alguma observação.
Também é importante destacar que o campo de observação deve ser opcional e o seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação"
}
```

### PUT /letterOfGuarantee/requests/consent

Dá anuância a uma solicitação de Carta-Fiança. O botão de ciência só deve aparecer para o diretor financeiro(permissão 64) e para o presidente(permissão 32) e pode aparecer a qualquer momento.

No modal com o formulário para aprovação, deve ser exibido um campo para o usuário fazer alguma observação.
Também é importante destacar que o campo de observação deve ser opcional e o seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "note": "Observação"
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

### PUT /letterOfGuarantee/requests/distribute

Distribui uma solicitação de Carta-Fiança. O botão `Distribuir...` só deve aparecer para advogado(permissão 2048) e só pode aparecer se a solicitação estiver no status SENT_TO_LEGAL_OPINION(10).

No modal com o formulário para distribuição, deve ser exibido um campo para o usuário fazer alguma observação e um campo para selecionar o advogado(deve ser buscado em `/letterOfGuarantee/lawyers`).

Também é importante destacar que o campo de observação deve ser opcional e o seu label deve ser "Observação".

Retorna status 204.

#### JSON de entrada
    
```json
{
    "id": "uuidv4",
    "note": "Observação",
    "lawyerID": "uuidv4"
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

### PUT /letterOfGuarantee/requests/{id}/requestDocument

Solicita um documento para um pedido de Carta-Fiança.
O status da Carta-Fiança mudará para `solicitado documento(23)`.
O botão de solicitar documento só deve aparecer para a secretária(permissão 1024).

#### Parâmetros

```json
{
    "note": "string"
}
```

### GET /letterOfGuarantee/filter

Retorna uma lista de solicitações de Carta-Fiança filtradas.
As variáveis de filtro são:
- start: Data de início da solicitação
- finish: Data de término da solicitação
- number: Número da solicitação
- name: Parte do nome ou CPF do solicitante

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
            "authorizeDiscount": true,
            "status": 2,
            "lessor": "João da Silva",
            "administrator": "Joaquim da Silva",
            "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
            "letterValue": 200000,
            "number": 23,
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
            "authorizeDiscount": true,
            "lessor": "João da Silva",
            "administrator": "Joaquim da Silva",
            "address": "Rua da Consolação, 123, Bairro da Consolação, São Paulo, SP",
            "letterValue": 200000,
            "status": 2,
            "number": 23,
            "requester": {
                "id": "uuidv4",
                "name": "João da Silva"
            }
        }
    ]
}
```

## Rotas para uploads de arquivos


### POST /letterOfGuarantee/requests/{id}/attachments/{type}

Adiciona um anexo a uma solicitação de carta-fiança. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64, podendo ser PDF, PNG ou JPG.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `5`: Contrato de locação
  - `6`: Carta-fiança assinada
  - `7`: Documento a ser anexado ao pedido de carta-fiança
  - `8`: Parecer jurídico

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

### GET /letterOfGuarantee/requests/{id}/attachments

Retorna os anexos de uma solicitação de carta-fiança. Os arquivos podem ser baixados através do link `/files/{attachmentID}`.

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

### DELETE /attachments/{id}

Remove um anexo de uma solicitação de carta-fiança. O botão de remoção só deve aparecer para o autor da solicitação e para a secretária(permissão 1024). O botão de remoção não pode aparecer se a solicitação estiver nos status DOWNLOADED(14).

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário"
}
```