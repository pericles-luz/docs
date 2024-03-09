# Sistema para gerenciamento de propostas em eventos

Sistema a ser usado em eventos como CNRE e AGN para gerenciamento de propostas a serem apreciadas nos eventos.

## Premissas

- As propostas precisam estar associadas a um evento;
- As propostas precisam estar associadas a um autor;
- As propostas precisam estar associadas a uma unidade sindical;
- As propostas para eventos do tipo CNRE precisam constar em ata da unidade sindical;
- As propostas para eventos do tipo AGN podem ser associadas ao trecho do estatuto que pretendem mudar;
- As propostas para eventos do tipo AGN podem ser enviadas por qualquer pessoa;
- As propostas para eventos do tipo AGN podem ser do tipo estatutárias ou não estatutárias;

## Rotas para o sistema

- `/events/{eventID}/proposals` - Gerenciamento de propostas

### POST `/events/{eventID}/proposals`

Adiciona uma proposta. O botão para nova proposta só deve aparecer se o campo `canSendProposal` do evento estiver como `true`.

#### Parâmetros

```json
{
  "unionUnitID": "uuidv4",
  "authorID": "uuidv4",
  "type": 1,
  "title": "string",
  "description": "string",
  "justification": "string",
}
```

- `eventID`: ID do evento
- `unionUnitID`: ID da unidade sindical representada pela proposta. Deve ser buscada em `GET /events/{eventID}/proposals/unionUnits`
- `authorID`: ID do autor da proposta. Deve ser buscada em `/persons/find`
- `type`: tipo da proposta. Se evento for do tipo AGN(1), pode ser estatutária(1) ou não estatutária(2). Se evento for do tipo CNRE(2), deve ser constante em ata(3)
- `title`: título da proposta
- `description`: descrição da proposta. Deve ser um componente que permita formatação de texto com negrito, itálico e sublinhado.
- `justification`: justificativa da proposta. Deve ser um componente que permita formatação de texto com negrito, itálico e sublinhado.

#### Retorno

```json
{
  "id": "uuidv4",
  "eventID": "uuidv4",
  "number": "string",
  "unionUnit": {
    "id": "uuidv4",
    "name": "string"
  },
  "author": {
    "id": "uuidv4",
    "name": "string"
  },
  "type": 1,
  "title": "string",
  "description": "string",
  "justification": "string",
  "status": 1
}
```

### PUT `/events/proposals/{proposalID}`

Atualiza uma proposta. O botão para editar proposta só deve aparecer se o campo `canSendProposal` do evento estiver como `true`.

#### Parâmetros

```json
{
  "unionUnitID": "uuidv4",
  "type": 1,
  "title": "string",
  "description": "string",
  "justification": "string",
}
```

#### Retorno

```json
{
  "id": "uuidv4",
  "eventID": "uuidv4",
  "number": "string",
  "unionUnit": {
    "id": "uuidv4",
    "name": "string"
  },
  "author": {
    "id": "uuidv4",
    "name": "string"
  },
  "type": 1,
  "title": "string",
  "description": "string",
  "justification": "string",
  "status": 1
}
```

### GET `/events/{eventID}/proposals`

Lista todas as propostas de um evento. Pode ser filtrada por número de proposta, título, autor e unidade sindical.

#### Parâmetros

- `eventID`: ID do evento
- `number`: número da proposta
- `title`: parte do título da proposta
- `author`: parte do nome do autor
- `unionUnitID`: id da unidade sindical

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "number": "string",
    "eventID": "uuidv4",
    "unionUnit": {
        "id": "uuidv4",
        "name": "string"
    },
    "author": {
        "id": "uuidv4",
        "name": "string"
    },
    "title": "string",
    "status": 1
  }
]
```

### GET `/events/proposals/{proposalID}`

Retorna uma proposta.

#### Retorno

```json
{
  "id": "uuidv4",
  "eventID": "uuidv4",
  "number": "string",
  "unionUnit": {
    "id": "uuidv4",
    "name": "string"
  },
  "author": {
    "id": "uuidv4",
    "name": "string"
  },
  "type": 1,
  "title": "string",
  "description": "string",
  "justification": "string",
  "status": 1
}
```

### GET `/events/{eventID}/proposals/unionUnits`

Lista todas as unidades sindicais que podem ser associadas a uma proposta feita pelo usuário.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "name": "string"
  }
]
```

### DELETE `/events/proposals/{proposalID}`

Apaga uma proposta. Só pode ser usada se a proposta estiver no status criada(1) ou submetida(2).

#### Parâmetros

{
    "note": "string"
}

- `note`: justifivativa obrigatória para apagar a proposta

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/events/proposals/{proposalID}/submit`

Submete uma proposta. Só pode ser usada se a proposta estiver no status criada(1).

#### Parâmetros

```json
{
  "note": "string"
}
```

- `note`: nota que será enviada para o autor da proposta

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/events/proposals/{proposalID}/approve`

Aprova uma proposta. Só pode ser usada se a proposta estiver no status submetida(2).

#### Parâmetros

```json
{
  "note": "string"
}
```

- `note`: observação opcional

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/events/proposals/{proposalID}/reject`

Rejeita uma proposta. Só pode ser usada se a proposta estiver no status submetida(2).

#### Parâmetros

```json
{
  "note": "string"
}
```

- `note`: observação obrigatória

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/events/proposals/{proposalID}/cancel`

Cancela uma proposta. Só pode ser usada se a proposta estiver no status submetida(2).

#### Parâmetros

```json
{
  "note": "string"
}
```

- `note`: observação obrigatória

#### Retorno

Retorna 204 se tudo der certo.

### GET `/events/proposals/{proposalID}/history`

Retorna o histórico de uma proposta.

