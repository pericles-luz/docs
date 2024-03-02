# Agendamento de reuniões

## Objetivo

O objetivo desta API é prover um serviço de agendamento de reuniões para que os filiados possam agendar reuniões com os dirigentes.

## Lista de Agendas

Lista de agendas disponíveis para agendamento. Deve mostrar ao usuário as agendas disponíveis para agendamento, em formato de detalhes que mostrem os campos retornados pela API.

Todos os usuários podem acessar esta funcionalidade.

Os campos `start` e `end` devem ser formatados para exibição no formato `DD/MM/YYYY a DD/MM/YYYY` e com a label `Disponível de`.

O campo `link` deve ser exibido como um botão com o texto `Agendar` que ao ser clicado, redireciona o usuário para o link da reunião.

### GET /schedulings

Lista as agendas disponíveis para agendamento.

#### Retorno

```json
[
  {
    "id": "uuidv4",
    "title": "Reunião com o Presidente",
    "description": "Reunião com o presidente para tratar de assuntos diversos",
    "start": "2021-10-01",
    "end": "2021-10-01",
    "link": "https://meet.google.com/abc-def-ghi"
  }
]
```

- `id` (string): Identificador único da agenda.
- `title` (string): Título da agenda.
- `details` (string): Descrição da agenda. Poderá ser HTML.
- `start` (string): Data de início da agenda no formato `YYYY-MM-DD`.
- `end` (string): Data de término da agenda no formato `YYYY-MM-DD`.
- `link` (string): Link para a reunião.