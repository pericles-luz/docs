# API para Rateio

## Objetivo

O objetivo desta API é permitir o gerenciamento e cálculo de rateios baseados no que prevê o estatuto do sindicato.

## Rateio

### POST /apportionments/next

Cria um novo rateio para o mês posterior ao último rateio existente. A criação do rateio já importa os dados de filiados e arrecadação.

Só é possível criar um novo rateio se o último rateio existente estiver finalizado.

#### JSON de entrada

```json
{
  "note": "Início dos descontos da AGN"
}
```

- `note`: Observação sobre o rateio, não é obrigatório ter um texto, mas o campo deve existir.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "name": "Rateio do mês 01-2020",
        "note": "Início dos descontos da AGN",
        "month": "01-2020",
        "reference": "12-2019",
        "status": 1
    }
}
```

- `id`: Identificador único do rateio
- `name`: Nome do rateio(gerado automaticamente)
- `note`: Observação sobre o rateio
- `month`: Mês do rateio
- `reference`: Mês de referência do rateio
- `status`: Status do rateio.

### GET /apportionments

Retorna todos os rateios existentes.

#### JSON de retorno

```json
{
    "data": [
        {
        "id": "uuidv4",
        "name": "Rateio do mês 01-2020",
        "note": "Início dos descontos da AGN",
        "month": "01-2020",
        "reference": "12-2019",
        "status": 100,
        },
        {
        "id": "uuidv4",
        "name": "Rateio do mês 02-2020",
        "note": "Início dos descontos da AGN",
        "month": "02-2020",
        "reference": "01-2020",
        "status": 100,
        }
    ]
}
```

### GET /apportionments/:id

Retorna um rateio específico.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "name": "Rateio do mês 01-2020",
        "note": "Início dos descontos da AGN",
        "month": "01-2020",
        "reference": "12-2019",
        "status": 100,
    }
}
```

### PUT /apportionments/:id

Atualiza um rateio específico.

#### JSON de entrada

```json
{
  "note": "Início dos descontos da AGN"
}
```

- `note`: Observação sobre o rateio, não é obrigatório ter um texto, mas o campo deve existir.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "name": "Rateio do mês 01-2020",
        "note": "Início dos descontos da AGN",
        "month": "01-2020",
        "reference": "12-2019",
        "status": 100,
    }
}
```

### DELETE /apportionments/:id

Deleta um rateio específico e retorna status 204, caso tudo ocorra bem.

## Descontos

### POST /discounts

Cria um novo desconto a ser considerado na geração dos rateios. Os descontos podem ser de vários tipos e os campos de entrada variam de acordo com o tipo de desconto.

#### JSON de entrada

```json
{
  "name": "Desconto da AGN",
  "type": "uuidv4",
  "unionUnitID": "uuidv4",
  "description": "Desconto da AGN",
  "percentage": 15,
  "value": 25000,
  "startDate": "2020-01-01",
  "installments": 10,
  "stateID": 1,
  "beforeStateTransfer": true,
  "needAuthorization": true
}
```

- `name`: Nome do desconto
- `type`: Tipo do desconto
- `unionUnitID`: Identificador único da unidade sindical a ter o desconto implementado(ou null para todas). Aplica-se apenas a descontos por unidade sindical.
- `description`: Descrição do desconto
- `percentage`: Percentual do desconto(caso o desconto seja do tipo percentual)
- `value`: Valor do desconto(caso o desconto seja do tipo valor fixo)
- `startDate`: Data de início do desconto
- `installments`: Quantidade de parcelas do desconto
- `stateID`: Identificador único do estado a ter o desconto implementado(ou zero para todos). Aplica-se apenas a descontos por estado.
- `beforeStateTransfer`: Se o desconto deve ser aplicado antes ou depois do repasse para o estado, quando se tratar de estado com mais de uma unidade sindical. Aplica-se apenas a descontos por estado.
- `needAuthorization`: Se o desconto precisa de autorização para ser aplicado. Por exemplo, antecipação de AGN precisa de autorização da Unidade Sindical.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "name": "Desconto da AGN",
        "type": "uuidv4",
        "unionUnitID": "uuidv4",
        "description": "Desconto da AGN",
        "percentage": 15,
        "value": 25000,
        "startDate": "2020-01-01",
        "installments": 10,
        "stateID": 1,
        "beforeStateTransfer": true,
        "needAuthorization": true
    }
}
```

### GET /discounts

Retorna todos os descontos existentes.

#### JSON de retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "name": "Desconto da AGN",
            "type": "uuidv4",
            "unionUnitID": "uuidv4",
            "description": "Desconto da AGN",
            "percentage": 15,
            "value": 25000,
            "startDate": "2020-01-01",
            "installments": 10,
            "stateID": 1,
            "beforeStateTransfer": true,
            "needAuthorization": true
        },
        {
            "id": "uuidv4",
            "name": "Desconto da AGN",
            "type": "uuidv4",
            "unionUnitID": "uuidv4",
            "description": "Desconto da AGN",
            "percentage": 15,
            "value": 25000,
            "startDate": "2020-01-01",
            "installments": 10,
            "stateID": 1,
            "beforeStateTransfer": true,
            "needAuthorization": true
        }
    ]
}
```

### GET /discounts/:id

Retorna um desconto específico.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "name": "Desconto da AGN",
        "type": "uuidv4",
        "unionUnitID": "uuidv4",
        "description": "Desconto da AGN",
        "percentage": 15,
        "value": 25000,
        "startDate": "2020-01-01",
        "installments": 10,
        "stateID": 1,
        "beforeStateTransfer": true,
        "needAuthorization": true
    }
}
```

### PUT /discounts/:id

Atualiza um desconto específico.

#### JSON de entrada

```json
{
  "name": "Desconto da AGN",
  "type": "uuidv4",
  "unionUnitID": "uuidv4",
  "description": "Desconto da AGN",
  "percentage": 15,
  "value": 25000,
  "startDate": "2020-01-01",
  "installments": 10,
  "stateID": 1,
  "beforeStateTransfer": true,
  "needAuthorization": true
}
```

### DELETE /discounts/:id

Deleta um desconto específico e retorna status 204, caso tudo ocorra bem.

### POST /discounts/:id/authorize

Autoriza um desconto específico.

#### JSON de entrada

```json
{
  "unionUnitID": "uuidv4",
  "stateID": 1
}
```

- `unionUnitID`: Identificador único da unidade sindical a ter o desconto implementado(ou null para todas). Aplica-se apenas a descontos por unidade sindical.
- `stateID`: Identificador único do estado a ter o desconto implementado(ou zero para todos). Aplica-se apenas a descontos por estado.

#### JSON de retorno

```json
{
    "message": "Desconto autorizado com sucesso"
}
```

### POST /discounts/:id/unauthorize

Desautoriza um desconto específico.

#### JSON de entrada

```json
{
  "unionUnitID": "uuidv4",
  "stateID": 1
}
```

- `unionUnitID`: Identificador único da unidade sindical a ter o desconto implementado(ou null para todas). Aplica-se apenas a descontos por unidade sindical.
- `stateID`: Identificador único do estado a ter o desconto implementado(ou zero para todos). Aplica-se apenas a descontos por estado.

#### JSON de retorno

```json
{
    "message": "Desconto desautorizado com sucesso"
}
```

## Mensalidades

Gerencia as mensalidades pagas de forma avulsa pelos filiados

### POST /monthlyPayments

Registra o pagamento de uma mensalidade para um filiado específico.

#### JSON de entrada

```json
{
  "memberID": "uuidv4",
  "value": 25000,
  "paymentDate": "2020-01-01"
}
```

- `value`: Valor da mensalidade
- `paymentDate`: Data do pagamento da mensalidade

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "memberID": "uuidv4",
        "value": 25000,
        "paymentDate": "2020-01-01"
    }
}
```

### GET /monthlyPayments

Retorna todas as mensalidades pagas.

#### JSON de retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "memberID": "uuidv4",
            "value": 25000,
            "paymentDate": "2020-01-01"
        },
        {
            "id": "uuidv4",
            "memberID": "uuidv4",
            "value": 25000,
            "paymentDate": "2020-01-01"
        }
    ]
}
```

### GET /monthlyPayments/:id

Retorna uma mensalidade específica.

#### JSON de retorno

```json
{
    "data": {
        "id": "uuidv4",
        "memberID": "uuidv4",
        "value": 25000,
        "paymentDate": "2020-01-01"
    }
}
```

### PUT /monthlyPayments/:id

Atualiza uma mensalidade específica.

#### JSON de entrada

```json
{
  "value": 25000,
  "paymentDate": "2020-01-01"
}
```

### DELETE /monthlyPayments/:id

Deleta uma mensalidade específica e retorna status 204, caso tudo ocorra bem.

## Despesas

Permite informação de despesas das unidades sindicais

### POST /apportionments/:id/expenses/upload

Permite o upload de um arquivo CSV contendo as despesas de uma unidade sindical.

#### Payload

- Arquivo CSV contendo as despesas das unidades sindicais

```csv
UNIDADE SINDICAL|SALÁRIOS|13º SALÁRIO|FÉRIAS|RECISÕES|INSS PATRONAL|INSS 13Âº SALARIO|FGTS|PIS|IR S/ SALARIOS|IR S/FÉRIAS|VALE ALIMENTAÇÃO/ REFEIÇÃO| VALE TRANSPORTE/COMBUSTIVEL |PLANO SAUDE|INSS AUTÔNOMOS|ENCARGOS (IRRF/ CSRFS/ ALUGUEL/PJ)|ENCARGOS SERVICOS PF|CARTÓRIO|SEGURO DELEGADOS|OUTRAS DESPESAS PAGAS PELA DEN|DESCONTOS AUTORIZADOS PELA UNIDADE|SALDO MES ANTERIOR||
802||||||||||||||112,36|||||1.554,94|0,00|||
803|1.758,81||||448,50||140,70|17,59|||710,00||1.160,52|89,89|56,16|||361,62||0,00|||
804|4.040,38||8.060,38||1.030,30||323,23|40,40|||710,00||2.195,15|||||180,81|1.945,84|0,00|||
805||||||||||||||67,42||||361,62||0,00|||
```

Esse é o formato atual do CSV de Despesas pagas, mas será alterado assim que tiver o modelo da exportação do sistema financeiro.

```csv
UNIDADE SINDICAL (CODIGO)| SALDO CONTA CORRENTE| SALDO INVESTIMENTO| SALDO EM CAIXA| DADOS VALIDADOS (0 = NÃO, 1 = PENDENTE, 2 = SIM)| DATA DA PRESTACAO DE CONTAS
802|0,00|0,00|0,00|1|
803|0,00|0,00|0,00|1|
804|0,00|0,00|0,00|1|
805|0,00|0,00|0,00|1|
805|0,00|0,00|0,00|1|
806|1672,83|42090,52|604,44|2|06/06/2023
```

Essse é o formato atual do CSV de Disponibilidades, mas será alterado assim que tiver o modelo da exportação do sistema financeiro.

#### JSON de retorno

```json
{
    "message": "Despesas importadas com sucesso"
}
```
