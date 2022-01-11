<h1 align="center">
  <img alt="KenzieHub" title="KenzieHub" src="https://kenzie.com.br/images/logoblue.svg" width="100px" />
</h1>

<h1 align="center">
  CarLovers - API
</h1>

<p align = "center">
Este é o backend de CarLovers - Um hub dos que gostam de carros em podem cadastrar seus veículos e participar de uma rede postando seus comentários sobre assuntos relacionados.
</p>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

A API tem um total de 12 endpoints, sendo em volta principalmente do usuário (CarLover) - podendo cadastrar seu perfil, veículos que possui e registrar comentários na rede para todos os usuários.
<br/>

<!-- O JSON para utilizar no Insomnia é este aqui -> https://kenziehub.s3.amazonaws.com/requests_kenziehub.json
Para importar o JSON no Insomnia é só clicar na palavra "Insomnia" no canto superior esquerdo. Nesse dropdown é só clicar em "Import / Export > Import Data > From Url" e colocar o link acima :v:

O url base da API é https://kenziehub.herokuapp.com -->

## Rotas que não precisam de autenticação

<h2 align ='center'> Listando comentários </h2>

Aqui todos é possível visualizar todos os comentários feito por todos os usuários sem estar logado.

`GET /comments - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "text": "Acidente de transito com quem não tem seguro é foda",
    "date": "19-03-2020",
    "userId": 1,
    "id": 1
  },
  {
    "text": "Hoje eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 2
  }
]
```

Podemos utilizar os query params para mudar a lista. Com o parâmetro userId, podemos filtrar por usuário.

`GET /comments?userId=2 - FORMATO DA RESPOSTA - STATUS 200`

````json
[
  {
    "text": "Amanhã eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 2
  },
  {
    "text": "Amanhã eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 3
  },
  {
    "text": "Amanhã eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 4
  },
  {
    "text": "Amanhã eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 5
  },
  {
    "text": "Amanhã eu vou pescar",
    "date": "19-03-2021",
    "userId": 2,
    "id": 6
  }
]

<h2 align ='center'> Criação de usuário </h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
      "email": "monica@yahoo.com.br",
      "password": "123456",
      "name": "Monica",
      "age": 35
    }
````

Caso dê tudo certo, a resposta será assim:

`POST /users - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1vbmljYUB5YWhvby5jb20uYnIiLCJpYXQiOjE2NDE5MDAxNjIsImV4cCI6MTY0MTkwMzc2Miwic3ViIjoiMiJ9.sS9466Xd8_n30fU7gThr7xtnWQcAGi0F9M_Hh02Ya6I",
  "user": {
    "email": "monica@yahoo.com.br",
    "name": "Monica",
    "age": 35,
    "id": 2
  }
}
```

1. Os campos email e password são obrigatórios para a requisição.

2. A API já retorna o token no momento do cadastro, podendo-se dispensar o usuário voltar para a página de login.

<h2 align ='center'> Possíveis erros </h2>

Caso você acabe errando e mandando algum campo errado, a resposta de erro será assim:

Necessário ter email e senha.

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email and password are required"
```

Necessário formato de email válido.

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email format is invalid"
```

Email já cadastrado:

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email already exists"
```

<h2 align = "center"> Login </h2>

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "monica@yahoo.com.br",
  "password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1vbmljYUB5YWhvby5jb20uYnIiLCJpYXQiOjE2NDE5MDQyMTAsImV4cCI6MTY0MTkwNzgxMCwic3ViIjoiMiJ9.VlXStsfP2m3PxELxON0aYh2bAq6IRL7FEymeo0Vvg_I",
  "user": {
    "email": "monica@yahoo.com.br",
    "name": "Monica",
    "age": 4,
    "id": 2,
    "userId": 2
  }
}
```

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}
> Após o usuário estar logado, ele deve conseguir cadastrar cometários é os veículos que possui.

<h2 align ='center'> Criar comentários </h2>

`POST /comments - FORMATO DA REQUISIÇÃO`

```json
{
  "text": "Amanhã eu vou pescar",
  "date": "19-03-2021",
  "userId": 2
}
```

1. O campo - "text" recebe os comentário do usuário sobre o tema relacionado a carros.
2. É nessário ifnormar o campo userId, pois somente o usuário que criou terá acesso para fazer alterções:

`PATCH /comments/:comment_id - FORMATO DA REQUISIÇÃO`

```json
{
  "text": "Hoje eu vou pescar",
  "date": "19-03-2021",
  "userId": 2
}
```

3. Caso não informe o userId receberá o seguinte erro:

Ou seja, você pode apenas dar update em quanto você avançou nas tecnologias que já está no seu perfil. Utilizando este endpoint:

```json
"Private resource creation: request body must have a reference to the owner id"
```

Também é possível deletar um commentário, utilizando este endpoint, desde que seja um comentário do próprio usuário:

`DELETE /comments/:comment_id`

```
Não é necessário um corpo da requisição.
```

<h2 align ='center'> Cadastrar carros </h2>

Da mesma forma de criar comentários, conseguimos cadastrar veículos, dessa forma:

`POST /cars - FORMATO DA REQUISIÇÃO`

```json
{
  "Marca": "Toyota",
  "Modelo": "Etios",
  "userId": 2
}
```

Conseguimos atualizar a Marca e o Modelo utilizando este endpoint:

`PATCH /cars/:car_id - FORMATO DA REQUISIÇÃO`

```json
{
  "Marca": "Renault",
  "Modelo": "Sandero",
  "userId": 2
}
```

Também é possível deletar um trabalho do seu perfil, utilizando este endpoint:

`DELETE /cars/:car_id`

```
Não é necessário um corpo da requisição.
```

A busca de todos os veículos cadastrados só pode ser consultada pelo próprio usuário. O formato da requisição é:

`DELETE /cars`

```
Não é necessário um corpo da requisição.
```

<h2 align ='center'> Atualizando os dados do perfil </h2>

Assim como os endpoints de Comments e Cars, nesse precisamos estar logados, com o token no cabeçalho da requisição. Estes endpoints são para atualizar seus dados do usuário como senha e mais informações.

Endpoint para atualizar um dado específico do usuário. É possível fazer alteração por outro email válido que não esteja sendo utilizado ou da senha:

`PATCH /users/:user_id - FORMATO DA REQUISIÇÃO`

```json
{
  "age": 15
}
```

Nesse endpoint é possível modificar o perfil do usuário em sua totalidade, incluindo ou excluindo propriedades, ou seja, o que não for incluído na requisição será deletado, com exceção do email e senha que são obrigatórios.

`PUT /users/:user_id - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "monica@yahoo.com.br",
  "password": "123456",
  "new_property": "new_property"
}
```

---

Daniel M. Satiro
