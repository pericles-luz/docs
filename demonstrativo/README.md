# API para demonstrativo de IRPF

## Objetivo

O objetivo desta API é fornecer os demonstrativos de IRPF, refferentes ao recebimento de precatórios.

## Endpoints

### GET /irpf/:cpf

Lista todas as ações para as quais o usuário tenha recebido precatórios.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "name": "Ação 1",
    "owner": {
      "id": "uuidv4",
      "name": "Joaquim da Silva"
    },
    "hasIRPF": true
  },
  {
    "id": "uuidv4",
    "name": "Ação 2",
    "owner": {
      "id": "uuidv4",
      "name": "Joaquim da Silva"
    },
    "hasIRPF": false
  }
]
```

- `id`: Identificador da ação
- `name`: Nome da ação
- `hasIRPF`: Indica se o usuário tem demonstrativo de IRPF para esta ação
- `owner`: Dados do proprietário da ação
  - `id`: Identificador do proprietário
  - `name`: Nome do proprietário

### GET /irpf/find/:cpfOrName

Lista todas as ações para as quais o usuário tenha recebido precatórios.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "name": "Ação 1",
    "owner": {
      "id": "uuidv4",
      "name": "Joaquim da Silva"
    },
    "hasIRPF": true
  },
  {
    "id": "uuidv4",
    "name": "Ação 2",
    "owner": {
      "id": "uuidv4",
      "name": "Joaquim da Silva"
    },
    "hasIRPF": false
  }
]
```

- `id`: Identificador da ação
- `name`: Nome da ação
- `hasIRPF`: Indica se o usuário tem demonstrativo de IRPF para esta ação
- `owner`: Dados do proprietário da ação
  - `id`: Identificador do proprietário
  - `name`: Nome do proprietário

### GET /irpf/:id/:cpf/data

Retorna os dados para o demonstrativo de IRPF de uma pessoa.

#### Retorno

```json
{
  "id": 1,
  "name": "Ação 1",
  "months": 12,
  "courtOrder": "0243619-47.2021.4.01.9198",
  "main": 200000,
  "interest": 10000,
  "issuanceStart": "2021-01-01",
  "issuanceEnd": "2021-12-31",
  "payment": "2021-12-31",
  "file": "/documents/00000000191/1/A2C542AE"
}
```

- `id`: Identificador da ação
- `name`: Nome da ação
- `courtOrder`: Número do precatório
- `issuanceStart`: Data de início do intervalo de expedição do precatório
- `issuanceEnd`: Data de fim do intervalo de expedição do precatório
- `payment`: Data de pagamento do precatório
- `months`: Número de meses
- `main`: Valor principal recebido
- `interest`: Valor de juros recebido
- `file`: URL para download do arquivo PDF com o demonstrativo de IRPF