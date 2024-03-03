# Sistema para votações em eventos

Sistema a ser usado em eventos como CNRE e AGN para votações instantâneas.

## Premissas

- Os participantes devem ser registrados no sistema com antecedência.
- Deve haver um de gerenciamento de participantes onde se define quem pode votar.
- Deve haver um gerenciamento de votações onde se define as perguntas e as opções de resposta.
- Deve haver um gerenciamento de votos onde se registra quem votou quem já votou e mostre um timer para a votação.
- Após a votação, deve-se mostrar o resultado.
- É possível haver votação anônima ou não.
- Apenas votações abertas podem ser votadas.
- Apenas votações fechadas podem ser encerradas.
- Apenas votações encerradas podem ser mostradas.
- Apenas votantes autorizados podem votar.

## Rotas para o sistema

- `/voting/{eventID}/voters` - Gerenciamento de votantes
- `/voting/{eventID}/questions` - Gerenciamento de perguntas
- `/voting/{eventID}/votings` - Gerenciamento de votações

### POST `/voting/{eventID}/voters`

Adiciona um votante.

#### Parâmetros

```json
{
  "eventID": "uuidv4",
  "unionUnitID": "uuidv4",
  "personID": "uuidv4",
  "location": 1,
  "position": 1,
}
```

- `eventID` - ID do evento
- `unionUnitID` - ID da unidade sindical representada pelo participante
- `personID` - ID do participante na tabela de pessoas
- `location` - localização do participante na votação (1 - presencial, 2 - remoto)
- `position` - condição do participante na votação (1 - titular, 2 - suplente)

#### Retorno

```json
{
  "voterID": "uuidv4",
  "unionUnitID": "uuidv4",
  "UnionUnitName": "string",
  "personID": "uuidv4",
  "personName": "string",
  "able": true,
  "location": 1,
  "position": 1
}
```

### PUT `/voting/voters/{voterID}`

Atualiza um votante. Será necessário ser usada sempre que um suplente assumir o lugar de um titular. Caberá ao usuário do sistema definir que titular será substituído e por quem. Assim, o usuário deve habilitar o suplente e desabilitar o titular.

#### Parâmetros

```json
{
  "able": true
}
```

- `voterID` - ID do votante
- `able` - true para habilitar, false para desabilitar

#### Retorno

```json
{
  "voterID": "uuidv4",
  "unionUnitID": "uuidv4",
  "UnionUnitName": "string",
  "personID": "uuidv4",
  "personName": "string",
  "able": true,
  "location": 1,
  "position": 1
}
```

### GET `/voting/{eventID}/voters`

Lista os votantes registrados para um evento.

#### Retorno

```json
[
    {
    "voterID": "uuidv4",
    "unionUnitID": "uuidv4",
    "UnionUnitName": "string",
    "personID": "uuidv4",
    "personName": "string",
    "able": true,
    "location": 1,
    "position": 1
    }
]
```

### POST `/voting/{eventID}/questions`

Adiciona uma pergunta a uma votação. As perguntas podem ser de três tipos:

- Padrão(1): Pergunta cujas opções de resposta são definidas pelo sistema. Favorável, Contrário e Abstenção são as opções padrão.
- Personalizada(2): Pergunta cujas opções de resposta são definidas pelo usuário.

#### Parâmetros

```json
{
  "eventID": "uuidv4",
  "title": "string",
  "question": "string",
  "anonymous": true,
  "type": 1,
  "choices": 1,
  "options": [
    {
      "option": "string",
      "color": "ff00ff",
      "order": 1
    }
  ]
}
```

- `eventID`: ID do evento
- `title`: título da pergunta que será mostrado na tela e no relatório final
- `question`: texto da pergunta que será mostrado na tela
- `anonymous`: true para votação anônima, false para votação nominal
- `type`: tipo da pergunta (1 - padrão, 2 - personalizada)
- `choices`: número de respostas possíveis. Pode ser de 1 ao número de opções disponíveis.
- `options`: opções de resposta para perguntas personalizadas
    - `option`: texto da opção
    - `color`: cor da opção em hexadecimal
    - `order`: ordem de exibição da opção

#### Retorno

```json
{
  "questionID": "uuidv4",
  "eventID": "uuidv4",
  "title": "string",
  "question": "string",
  "anonymous": true,
  "type": 1,
  "choices": 1,
  "statusID": 1,
  "options": [
    {
      "option": "string",
      "color": "ff00ff",
      "order": 1
    }
  ]
}
```

### PUT `/voting/questions/{questionID}`

Atualiza uma pergunta. Só pode ser usada se a pergunta estiver no status criada(1) ou pausada(20).

#### Parâmetros

```json
{
  "title": "string",
  "question": "string",
  "anonymous": true,
  "type": 1,
  "choices": 1,
  "options": [
    {
      "option": "string",
      "color": "ff00ff",
      "order": 1
    }
  ]
}
```

#### Retorno

```json
{
  "questionID": "uuidv4",
  "eventID": "uuidv4",
  "title": "string",
  "question": "string",
  "anonymous": true,
  "type": 1,
  "choices": 1,
  "statusID": 1,
  "options": [
    {
      "option": "string",
      "color": "ff00ff",
      "order": 1
    }
  ]
}
```

### GET `/voting/{eventID}/questions`

Lista as perguntas registradas para um evento.

#### Retorno

```json
[
  {
    "questionID": "uuidv4",
    "eventID": "uuidv4",
    "title": "string",
    "statusID": 1
  }
]
```

### GET `voting/questions/{questionID}`

Mostra uma pergunta específica.

#### Retorno

```json
{
  "questionID": "uuidv4",
  "eventID": "uuidv4",
  "title": "string",
  "question": "string",
  "anonymous": true,
  "type": 1,
  "choices": 1,
  "statusID": 1,
  "options": [
    {
      "option": "string",
      "color": "ff00ff",
      "order": 1
    }
  ]
}
```

### PUT `/voting/votings/{questionID}/open`

Abre uma votação para uma pergunta. Só pode ser usada se a pergunta estiver no status criada(1) ou pausada(20).

Não há parâmetros.

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/voting/votings/{questionID}/suspend`

Pausa uma votação para uma pergunta. Só pode ser usada se a pergunta estiver no status aberta(19).

Não há parâmetros.

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/voting/votings/{questionID}/close`

Fecha uma votação para uma pergunta. Só pode ser usada se a pergunta estiver no status aberta(19). Após fechar a votação, o sistema deve continuar na tela de votação até que o usuário decida encerrar.

Não há parâmetros.

#### Retorno

Retorna 204 se tudo der certo.

### PUT `/voting/votings/{questionID}/end`

Encerra uma votação para uma pergunta. Só pode ser usada se a pergunta estiver no status fechada(21). Após encerrar a votação, o sistema deve mostrar o resultado.

Não há parâmetros.

#### Retorno

Retorna 204 se tudo der certo.

### GET `/voting/votings/{questionID}/result`

Mostra o status de uma votação para uma pergunta. Se a votação estiver aberta ou pausada, deve retorna apenas o status e a lista de votantes. 

Se a votação estiver fechada, deve retornar o status, a lista de votantes e o resultado. Se a votação não for anônima, deve retornar as opções escolhidas por cada votante.

#### Retorno

```json
{
  "statusID": 0,
  "voters": [
    {
      "voterID": "uuidv4",
      "personName": "string",
      "options": ["uuidv4"]
    }
  ],
  "result": [
    {
      "option": "string",
      "votes": 1
    }
  ]
}
```

### GET `/voting/{eventID}/votings`

Lista as votações registradas para um evento.

#### Retorno

```json
[
  {
    "questionID": "uuidv4",
    "eventID": "uuidv4",
    "title": "string",
    "statusID": 1
  }
]
```

### POST `/voting/votings/{questionID}/vote`

Registra um voto para uma pergunta. Só pode ser usada se a pergunta estiver no status aberta(19).

#### Parâmetros

```json
{
  "options": ["uuidv4"]
}
```

- `options`: lista de opções escolhidas pelo votante, respeitado o número de escolhas permitidas.

#### Retorno

Retorna 204 se tudo der certo.

