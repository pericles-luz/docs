# Controle de presença em eventos

## Premissas

- Os participantes devem ser registrados no sistema com antecedência.
- A participação pode ser presencial ou remota.
- Deve haver um gerenciamento de participantes onde se define quem pode participar. Quem tiver permissão Evento(16384) pode habilitar ou desabilitar a participação de uma pessoa.
- No cadastro do evento é preciso definir a presença mínima para que a participação seja considerada válida.
- A funcionalidade de portaria deve ser capaz de registrar a presença de um participante a partir de seu CPF.
- A funcionalidade de portaria deve ser capaz de registrar a presença de um participante a partir de seu QRCode(preferido) ou código de barras.
- Deve haver um módulo para verificação de quem está presente.
- Deve haver um relatório de presença onde se mostra o precentual de presença e a lista de presentes.

### Cadastro de participantes

- Deve ter uma tela inicial onde se lista os eventos
- Após escolher o evento, deve aparecer uma lista com os participantes já cadastrados, mostrando nome, unidade sindical e tipo de participação.
- Deve haver um botão para adicionar participantes.
- Clicando num participante, deve ser aberto em detalhes o cadastro do participante.
- Deve haver um botão para excluir o participante nos detalhes.
- Deve haver um botão para editar o participante nos detalhes.
- Os dados para cadastro de um participante são:
    - `Nome` (obrigatório): buscado em `/pessoa/find`
    - `Unidade Sindical` (obrigatório): buscado em `GET /events/{eventID}/participants/unionUnits`
    - `Tipo de participação` (obrigatório): buscado em `GET /events/{eventID}/participants/participationTypes`
    - `Local de participação` (obrigatório): indica se a participação será presencial ou remota
    - `Observações` (opcional): campo de texto livre
- Deve haver um botão em detalhes para bloquear a participação do participante.
- Deve haver um botão em detalhes para desbloquear a participação do participante.
- Deve haver um botão na listagem de participantes para gerar etiquetas

### Portaria

- Deve ter uma tela onde são listados os últimos eventos de entrada e saída.
- Deve haver um botão para registrar a entrada de um participante de modo avulso, usando CPF.

### QRCode

- Deve haver um QRCode para que registre entrada e saída do participante.

### Verificação de presença

- Deve haver uma tela onde se lista os participantes presentes e ausentes.

### Relatório de presença

- Deve haver um relatório de presença onde se mostra o precentual de presença e a lista de participantes.
- Deve haver um botão para exportar o relatório em PDF.


## Rotas de API


### `GET /events/{eventID}/participants/unionUnits`

Retorna a lista de unidades sindicais que podem participar do evento.

#### Retorno

```json
{
    "data": [
        {
            "id": "uuidv4",
            "name": "Nome da unidade sindical"
        }
    ]
}
```
### `POST /events/{eventID}/participants`

Cadastra um participante no evento.

#### Parâmetros

```json
{
    "personId": "uuidv4",
    "unionUnitId": "uuidv4",
    "type": 1,
    "local": 1,
    "note": "Observações"
}
```

- `personId` (obrigatório): ID do participante
- `unionUnitId` (obrigatório): ID da unidade sindical
- `type` (obrigatório): ID do tipo de participação
- `local` (obrigatório): 1 - presencial, 2 - remoto
- `note` (opcional): Observações

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "personId": "uuidv4",
        "unionUnitId": "uuidv4",
        "type": 1,
        "local": 1,
        "note": "Observações"
    }
}
```

### `PUT /events/{eventID}/participants/{participantID}`

Edita um participante no evento.

#### Parâmetros

```json
{
    "data": {
        "personId": "uuidv4",
        "unionUnitId": "uuidv4",
        "type": 1,
        "local": 1,
        "note": "Observações"
    }
}
```

#### Retorno

```json
{
    "data": {
        "id": "uuidv4",
        "person": {
            "id": "uuidv4",
            "name": "Nome do participante"
        },
        "unionUnit": {
            "id": "uuidv4",
            "name": "Nome da unidade sindical"
        },
        "type": 1,
        "local": 1,
        "note": "Observações"
    }
}
```

### `DELETE /events/participants/{participantID}`

Exclui um participante no evento.

#### Retorno

Retorno 204 se tudo der certo.

### `GET /events/{eventID}/participats/types`

Retorna a lista de tipos de participação que podem participar do evento.

#### Retorno

```json
{
    "data": [
        {
            "id": 1,
            "name": "Nome do tipo de participação"
        }
    ]
}
```

### `PUT /events/participants/{participantID}/block`

Bloqueia a participação de um participante.

#### Parâmetros

```json
{
    "note": "Motivo do desbloqueio"
}
```

#### Parâmetros

```json
{
    "data": {
        "note": "Motivo do bloqueio"
    }
}
```
#### Retorno

Retorna 204 se tudo der certo.

### `PUT /events/participants/{participantID}/unblock`

Desbloqueia a participação de um participante.

#### Parâmetros

```json
{
    "note": "Motivo do desbloqueio"
}
```

#### Retorno

Retorna 204 se tudo der certo.

### `GET /events/{eventID}/presences`

Retorna a lista de participantes do evento com a indicação de presença.

#### Retorno

```json
{
    "data": [
        {
            "personId": "uuidv4",
            "person": {
                "id": "uuidv4",
                "name": "Nome do participante"
            },
            "unionUnit": {
                "id": "uuidv4",
                "name": "Nome da unidade sindical"
            },
            "type": {
                "id": 1,
                "name": "Nome do tipo de participação"
            },
            "local": {
                "id": 1,
                "name": "Presencial"
            },
            "status": {
                "id": 1,
                "name": "Presente"
            }
        }
    ]
}
```

### `GET /events/{eventID}/presences/logs`

Retorna a lista de logs de presença do evento, em ordem decrescente.

#### Retorno

```json
{
    "data": [
        {
            "person": {
                "id": "uuidv4",
                "name": "Nome do participante"
            },
            "status": {
                "id": 1,
                "description": "Entrada"
            },
            "date": "2021-01-01T00:00:00Z"
        }
    ]
}
```

### `GET /events/{eventID}/labels`

Retorna PDF com etiquetas dos participantes.