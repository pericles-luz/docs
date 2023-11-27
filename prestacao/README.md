# API para prestação de contas das unidades sindicais

## Objetivo

O objetivo desta API é permitir que as unidades sindicais possam prestar contas de suas atividades, de forma a atender as exigências legais e estatutárias.

A ideia é que, no front-end, haja um dashboard para onde o usuário veja um gráfico com dados brutos da prestação atual e uma tabela onde serão apresentadas as classificações e seus valores consolidados.

Nesta tabela deve haver três colunas:

- Uma com a descrição da classificação;
- Uma com o valor consolidado da classificação;
- Uma coluna de ações, onde haverá um botão Abrir..., que abrirá um modal com o wizard, já na página desta classificação.

Também haverá um combobox onde será possível selecionar a prestação que se deseja visualizar.

## Wizard

O wizard é a forma de preenchimento da prestação. Ele é composto por uma série de páginas, onde cada página representa uma classificação. Cada página do wizard deve ter:

- Um título, que é o nome da classificação;
- Um botão Adicionar, que abrirá um modal com o formulário para adicionar um novo item. Este botão só deve aparecer se a classificação for editável e se a prestação não estiver fechada;
- Uma tabela, onde serão apresentados os itens da classificação(despesas e receitas);
- Um texto com explicações detalhadas sobre a classificação;
- Botões Anterior, Próximo e Fechar.

Na tabela, deve haver três colunas:

- Uma com a descrição do item;
- Uma com o valor do item;
- Uma coluna de ações, onde haverá um botão Editar, que abrirá um modal com o formulário para editar o item, e um botão Excluir, que excluirá o item.

## Formulário de inclusão e edição de item

O formulário de inclusão e edição de item deve ter:

- Um campo de seleção, onde o usuário poderá selecionar a classificação do item;
- Um campo de data, onde o usuário poderá selecionar a data de vencimento do item;
- Um campo de data, onde o usuário poderá selecionar a data de pagamento do item;
- Um campo de texto, onde o usuário poderá digitar a descrição do item;
- Um campo de valor, onde o usuário poderá digitar o valor do item;
- Uma área para gerenciar os anexos do item, onde o usuário poderá adicionar, visualizar e excluir anexos.

## Observações

- O modal deve seguir o padrão do modal de novo item de requisições;
- O campo de seleção de prestação deve seguir o padrão do campo de seleção de Rateio;
- O campo de seleção de classificação deve seguir o padrão do campo de seleção de Cidade do item de requisição;
- O campo de data deve seguir o padrão do campo de data de AGNU;
- O gerenciamento de upload deve seguir o padrão do gerenciamento de upload de item de requisição.

## Endpoints da API

### GET /accountabilities

Retorna uma lista de prestação de contas disponíveis para o usuário.

#### Retorno

```json
{
    "data": [
        {
            "id": 1,
            "description": "Prestação de contas 2",
            "date": 2020-10-01,
            "statusID": 1,
            "status": "open"
        },
        {
            "id": 2,
            "description": "Prestação de contas 1",
            "year": 2020-09-01,
            "statusID": 100,
            "status": "closed"
        }
    ]
}
```

### GET /accountabilities/classifications

Retorna uma lista de classificações disponíveis para o usuário.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "order": 1,
            "editable": true,
            "name": "Telefonia e fax",
            "description": "Todas as contas de telefonia e fax pagas pela unidade sindical devem ser informadas nesta classificação."
        },
        {
            "id": "uuidv4",
            "order": 2,
            "editable": true,
            "name": "Deslocamento",
            "description": "Todas as despesas com deslocamento pagas pela unidade sindical devem ser informadas nesta classificação. Táxi, Uber etc."
        }
    ]
}
```

### GET /accountabilities/:id/classifications

Retorna a lista de classificações para a prestação passada como parâmetro. A lista servirá para montar a tabela do dashboard.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "order": 1,
            "name": "Telefonia e fax",
            "description": "Todas as contas de telefonia e fax pagas pela unidade sindical devem ser informadas nesta classificação.",
            "editable": true,
            "total": 100000
        },
        {
            "id": "uuidv4",
            "order": 2,
            "name": "Deslocamento",
            "description": "Todas as despesas com deslocamento pagas pela unidade sindical devem ser informadas nesta classificação. Táxi, Uber etc.",
            "editable": true,
            "total": 200000
        }
    ]
}
```

### POST /accountabilities/:id/items

Adiciona um novo item à classificação passada como parâmetro. Essa rota só pode ser usada se a classificação for editável.

#### Parâmetros

```json
{
  "classificationID": "uuidv4",
  "originID": "uuidv4",
  "dueDate": "2020-10-01",
  "paymentDate": "2020-10-01",
  "note": "Conta de telefone",
  "amount": 100000
}
```

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "classificationID": "uuidv4",
        "originID": "uuidv4",
        "statusID": 1,
        "status": "não analisada",
        "dueDate": "2020-10-01",
        "paymentDate": "2020-10-01",
        "note": "Conta de telefone",
        "amount": 100000
    }
}
```

- `originID` deve ser a conta corrente de onde sairá ou para onde irá o valor do item. Deve ser uma conta corrente da unidade sindical e pode ser buscada em `/accounts`.
- `classificationID` deve ser a classificação do item. Deve ser uma classificação da prestação e pode ser buscada em `/accountabilities/:id/classifications`.

### PUT /accountabilities/items/:id

Edita o item passado como parâmetro.

#### Parâmetros

```json
{
  "classificationID": "uuidv4",
  "originID": "uuidv4",
  "statusID": 1,
  "status": "não analisada",
  "dueDate": "2020-10-01",
  "paymentDate": "2020-10-01",
  "note": "Conta de telefone",
  "amount": 100000
}
```

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "classificationID": "uuidv4",
        "originID": "uuidv4",
        "statusID": 1,
        "status": "não analisada",
        "dueDate": "2020-10-01",
        "paymentDate": "2020-10-01",
        "note": "Conta de telefone",
        "amunt": 100000
    }
}
```

### GET /accountabilities/:id/classifications/:id/items

Retorna a lista de itens para a classificação passada como parâmetro. A lista servirá para montar a tabela do wizard.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "accaountabilityID": "uuidv4",
            "classificationID": "uuidv4",
            "originID": "uuidv4",
            "statusID": 1,
            "status": "não analisada",
            "dueDate": "2020-10-01",
            "paymentDate": "2020-10-01",
            "note": "Conta de telefone",
            "amount": 100000
        },
        {
            "id": "uuidv4",
            "accaountabilityID": "uuidv4",
            "classificationID": "uuidv4",
            "originID": "uuidv4",
            "statusID": 1,
            "status": "não analisada",
            "dueDate": "2020-10-01",
            "paymentDate": "2020-10-01",
            "note": "Conta de telefone",
            "amount": 100000
        }
    ]
}
```


### POST /accountabilities/:id/classifications/:id/attachments/{type}

Adiciona um anexo a um item de prestação de contas. Importante observar que o anexo deve ser enviado em base64.
Também é importante destacar que o payload não é um JSON, mas apenas o arquivo em base64, podendo ser PDF, PNG ou JPG.

- `type` (obrigatório): Tipo de anexo. Podem ser:
  - `7`: Comprovante de pagamento ou recebimento

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