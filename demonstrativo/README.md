# API para demonstrativo de IRPF

## Objetivo

O objetivo desta API é fornecer os demonstrativos de IRPF, refferentes ao recebimento de precatórios.

## Endpoints

### GET /irpf/:cpf

Lista todas as ações para as quais o usuário tenha recedido precatórios.

#### Retorno

```json
[
  {
    "id": 1,
    "name": "Ação 1",
    "hasIRPF": true
  },
  {
    "id": 2,
    "name": "Ação 2",
    "hasIRPF": false
  }
]
```

### GET /irpf/:id/:cpf/data

Retorna os dados para o demonstrativo de IRPF de uma pessoa.

#### Retorno

```json
{
  "id": 1,
  "name": "Ação 1",
  "main": 200000,
  "interest": 10000
}
```