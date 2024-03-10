# Módulo responsável por gerenciar o cadastro do usuário

## Premissas

- Devem ser mostrados quadros separados para CRUD de dados cadastrais;
- O usuário deve ser capaz de adicionar e excluir dados cadastrais;
- Os dados nos quadros serão e-mail, telefone, endereço e conta bancária;
- Também será possível editar localização física e nome;
- Quem tiver permissão Cadastro(8192) poderá buscar por filiado e o select deve buscar por nome ou CPF;


## Rotas de API para gerenciar cadastro

## Telefone

### GET /phonenumbers

Retorna todos os telefones cadastrados para o usuário.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "number": "61912345678",
    "isMain": true
  }
]
```

### POST /phonenumbers

Adiciona um novo telefone para o usuário.

#### Parâmetros

```json
{
  "phonenumber": "61912345678",
  "isMain": true
}
```

- `phonenumber`: número de telefone com DDD.
- `isMain`: indica se o telefone é o principal do usuário. Deixar marcado por padrão

#### Retorno

```json
{
  "id": "uuidv4",
  "number": "61912345678",
    "isMain": true
}
```

### DELETE /phonenumbers/:id

Remove um telefone do usuário.

#### Retorno

Retorna 204 se o telefone foi removido com sucesso.

## E-mail

### GET /emails

Retorna todos os e-mails cadastrados para o usuário.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "email": "teste@testando.com",
    "isMain": true
  }
]
```

### POST /emails

Adiciona um novo e-mail para o usuário.

#### Parâmetros

```json
{
    "email": "teste@testando.com",
    "isMain": true
}
```

- `email`: endereço de e-mail.
- `isMain`: indica se o e-mail é o principal do usuário. Deixar marcado por padrão.

#### Retorno

```json
{
    "id": "uuidv4",
    "email": "teste@testando.com",
    "isMain": true
}
```

### DELETE /emails/:id

Remove um e-mail do usuário.

#### Retorno

Retorna 204 se o e-mail foi removido com sucesso.
