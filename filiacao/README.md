# API para cofirmação de filiação

## Objetivo

O objetivo desta API é possibilitar a confirmação de filiação de um pré-filiado.

Haverá no front-end uma lista de pré-filiados que ainda não tiveram sua filiação confirmada. O usuário poderá selecionar um pré-filiado e confirmar sua filiação.

Na listagem inicial, devem aparecer uuid, CPF e nome do pré-filiado. Ao selecionar um pré-filiado, deve aparecer uma tela com os dados do pré-filiado e um botão para confirmar a filiação.

Devem haver também um botão para editar os dados do pré-filiado e um botão para cancelar a filiação.

Deve ser criada, no menu lateral, uma área com o nome Cadastro(visível para todos) e a opção de Confirmação de Filiação(visível para quem tem permissão DTI(4096)). A opção de Confirmação de Filiação deve levar para a listagem de pré-filiados. 

- Antes de cancelar a filiação, deve ser mostrado um modal de confirmação.
- Antes de confirmar a filiação, deve ser mostrado um modal de confirmação.

## Endpoints

### GET /affiliations/pre-affiliations

Retorna uma lista de pré-filiados.

#### Retorno

```json
{
    "data": [
        {
            "id": "uidv4",
            "cpf": "string",
            "name": "string"
        }
    ]
}
```

### GET /affiliations/pre-affiliations/{id}

Retorna os dados de um pré-filiado.

#### Retorno

```json
{
    "data": {
        "id": "uidv4",
        "cpf": "string",
        "siape": "string",
        "birthDate": "aaaa-mm-dd",
        "name": "string",
        "email": "string",
        "phone": "string",
        "address": "string",
        "number": "string",
        "complement": "string",
        "neighborhood": "string",
        "city": "string",
        "state": "string",
        "zipcode": "string",
        "employeeStatus": 1,
        "physicalLocation": 1,
        "assignment": 1,
        "office": 1
    }
}
```

- `siape` é o número de matrícula do servidor e precisa ter 9 dígitos. A label deve ser "Siape (9 dígitos)". O campo é obrigatório.
- `employmentStatus` é o status do servidor, A label deve ser "Situação funcional". Os valores possíveis são:
    - 1 - Ativo
    - 2 - Aposentado
- `physicalLocation` é a localização física do servidor. Os valores possíveis devem ser buscados em `/physicalLocations`. O campo é obrigatório para servidores Ativos(`employmentStatus` igual a `1`).
- `assignment` é a lotação do servidor. Os valores possíveis devem ser buscados em `/locations`. O campo é obrigatório para servidores Ativos(`employmentStatus` igual a `1`). A label deve ser "Lotação".
- `office` é o exercício do servidor. Os valores possíveis devem ser buscados em `/locations`. O campo é obrigatório para servidores Ativos(`employmentStatus` igual a `1`). A label deve ser "Exercício".
- Todos os campos são obrigatórios, exceto `complement`, independente do valor de `employmentStatus`.

### PUT /affiliations/pre-affiliations/{id}

Atualiza os dados de um pré-filiado.

#### Parâmetros

```json
{
    "cpf": "string",
    "siape": "string",
    "birthDate": "aaaa-mm-dd",
    "name": "string",
    "email": "string",
    "phone": "string",
    "address": "string",
    "number": "string",
    "complement": "string",
    "neighborhood": "string",
    "city": "string",
    "state": "string",
    "zipcode": "string",
    "employeeStatus": 1,
    "physicalLocation": 1,
    "assignment": 1,
    "office": 1
}
```

#### Retorno

```json
{
    "data": {
        "id": "uidv4",
        "cpf": "string",
        "siape": "string",
        "birthDate": "aaaa-mm-dd",
        "name": "string",
        "email": "string",
        "phone": "string",
        "address": "string",
        "number": "string",
        "complement": "string",
        "neighborhood": "string",
        "city": "string",
        "state": "string",
        "zipcode": "string",
        "employeeStatus": 1,
        "physicalLocation": 1,
        "assignment": 1,
        "office": 1
    },
    "message": "string"
}
```

- Se houver algo em `message`, deve ser mostrado ao usuário.

### PUT /affiliations/pre-affiliations/{id}/cancel

#### Parâmetros

```json
{
    "note": "string"
}
```

Cancela a filiação de um pré-filiado.

#### Retorno

O status do retorno deve ser 204.

### POST /affiliations/pre-affiliations/{id}/confirm

Confirma a filiação de um pré-filiado.

#### Parâmetros

```json
{
    "note": "string"
}
```

- `note` é uma observação sobre a filiação. O campo é opcional.