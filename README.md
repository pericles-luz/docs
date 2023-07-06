# docs
Documentações


## Lista de API

- [AGNU](agnu/README.md)
- [Ressarcimento](ressarcimento/README.md)
- [Viagem](viagem/README.md)


## Rotas de autenticação

### POST /auth/token

Envia um token de acesso para o usuário. O token é enviado por SMS ou e-mail, dependendo do valor de `meioEnvio`.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "media": 1
}
```

- `cpf`: CPF do usuário
- `media`: Meio de envio do token de acesso. 1 para SMS, 2 para e-mail.

#### JSON de retorno

```json
{
  "message": "texto a ser exibido ao usuário",
}
```


### POST /auth

Autentica o usuário com o token de acesso.

#### JSON de entrada

```json
{
  "cpf": "00000000191",
  "token": "123456"
}
```

- `cpf`: CPF do usuário
- `token`: Token de acesso enviado ao usuário

#### JSON de retorno

```json
{
  "bearerToken": "JWT",
  "mensage": "texto a ser exibido ao usuário"
}
```

- `bearerToken`: Token de acesso para as demais rotas
- `mensage`: Texto a ser exibido ao usuário

O token de acesso é válido por 24 horas e seu payload será:

```json
{
  "exp": 1234567890,
  "iss": "sindireceita.org.br",
  "data": {
    "cpf": "00000000191",
    "nome": "Nome do usuário",
    "unidadeSindical": {
      "id": "65083027-c978-498d-974b-9ee04baf7b35",
      "nome": "Nome da unidade sindical"
    },
    "permissao": 1
  }
}
```

- `exp`: Data de expiração do token
- `dados`: Dados do usuário
- `permissao`: Permissão do usuário é um número `int64`. Cada bit do número representa uma permissão do sistema. As permissões são:
  - 1: Usuário - podee acessar o sistema e fazer requisições
  - 2: Filiado - pode acessar áreas restritas a filiados
  - 4: Servidor - pode acessar áreas restritas a servidores
  - 8: Aprovador - pode aprovar requisições
  - 16: Pagador - pode informar que o pagamento foi realizado
  - 32: Dirigente sindical - pode acessar áreas restritas a dirigentes sindicais
  - 64: Financeiro - pode acessar áreas restritas ao financeiro
  - 128: Comprador - pode informar a compra de passagens e reservas em hotéis


Para confirmar se um usuário tem uma permissão, basta fazer a operação `permissao & permissaoDesejada` e verificar se o resultado é diferente de zero. Por exemplo, para verificar se o usuário tem permissão de usuário, basta fazer `permissao & 1` e verificar se o resultado é diferente de zero. Em javascript, seria algo assim:

```javascript
const PERMISSAO_USUARIO = 1
if (permissao & PERMISSAO_USUARIO) {
  // usuário tem permissão de usuário
}
```

### GET /auth/certificate

Autentica o usuário com o certificado digital.

#### O JSON de retorno é o mesmo do POST /auth
