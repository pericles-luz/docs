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
    "hasIRPF": true
  },
  {
    "id": "uuidv4",
    "name": "Ação 2",
    "hasIRPF": false
  }
]
```

- `id`: Identificador da ação
- `name`: Nome da ação
- `hasIRPF`: Indica se o usuário tem demonstrativo de IRPF para esta ação

### GET /irpf/:id/:cpf/data

Retorna os dados para o demonstrativo de IRPF de uma pessoa.

#### Retorno

```json
{
  "id": 1,
  "name": "Ação 1",
  "main": 200000,
  "interest": 10000,
  "file": "/documents/00000000191/1/A2C542AE"
}
```

- `id`: Identificador da ação
- `name`: Nome da ação
- `main`: Valor principal recebido
- `interest`: Valor de juros recebido
- `file`: URL para download do arquivo PDF com o demonstrativo de IRPF