# relativi backend</br>

Este é o backend (json.server Fake API) da plataforma relativi.

<sup>This is the backend (json.server Fake API) of the relativi platform.</sup>


## Endpoints
Existem cinco endpoints: ```/users``` para criar um novo user, ```/login``` para conseguir um access token para o user, ```/prousers``` para criar um perfil público para professionais**, ```/profiles``` para criar um perfil público para usuários**, e ```/activities``` para criar uma nova atividade.

<sup>There are five endpoints: ```/users``` to create a new user, ```/login``` to get an access token for the user, ```/prousers``` to create a puclic** profile for professionals, ```/profiles``` to create a public** profile for users, and ```/activities``` to create a new activity.</sup>

** conteúdo pode ser acessado com qualquer token, porém só pode ser editado com o token de quem criou

<sup>** the content can read with any token, but can only be edite by the creator token</sup>

> **BASE URI:** 

```https://rel-ativi.herokuapp.com/```

- [User](#User)
- [Login](#Login)
- [Prousers](#Prousers)
- [Profiles](#Profiles)
- [Activities](#activities)

# [/users](#User)

Essa rota exclusividade de proprietário, ou seja, tudo que for criado nela é acessível apenas para quem criou. Então ela é não será utilizada para mostar o perfil de um professor para um usuário (ou vice-versa) pois um não teria acesso às infos do outro por essa rota.

## **Métodos:**

### POST -> Cria um novo usuário:
> **url.../users**

corpo do request (o que vai ser enviado):

```
{
"email": "email@email.com", /* obrigatório */
"password": "senha"  /* obrigatório */
"name": "nome"  /* regra de negócio */
"type": "tipo do usuário"   /* regra de negócio, "usuário" || "profissional" */
}
```

corpo da resposta (o que vai ser recebido):

```
{
"email": "email@email.com", 
"password": "senha"
"name": "nome"
"type": "tipo do usuário"   
"id": número /* gerado pela api*/
}
```

### GET -> Acessa um usuário: 
**precisa de token**
> **url.../users/:id**

corpo da resposta (o que vai ser recebido):

```
{
"email": "email@email.com",
"password": "senha"  /* criptografada não se assutem pois virá diferente do que colocarem na hora de criar */
"name": "nome"
"type": "tipo do usuário" 
"id": número /* gerado pela api*/
}
```

### PATCH -> Edita um usuário 
**precisa de token**
> **url.../users/:id**

corpo da resposta (o que vai ser enviado):

```
{
"email": "email@diferente.com"
/* apenas o atributo alterado precisa ser enviado, os que não foram alterados não */
}
```

### DELETE -> Exclui um usuário 
**precisa de token**
> **url.../users/:id**

Não é necessário enviar nada.


# [/prousers](#Prousers)
Essa rota tem exclusividade de token, ou seja, tudo que for criado nela é acessível apenas para quem tem token, não precisa ser o dono para poder ver, mas precisa ser o dono para editar.
Então será utilizada para mostar o perfil de um professional para um usuário, e deverá ser associada apenas com um usuário do tipo _profissional_

Minha recomendação é que essa associação deve ocorrer no momento do cadastro: form. preenchido com o tipo de conta "profissional" -> requisição na rota /users -> resposta positiva -> utiliza **alguns** dos dados para fazer uma segunda requisição para a essa rota (/prousers). 
Assim o processo já fica automatizado.

## **Métodos:**

### POST -> Cria um novo perfil profissional:
**precisa de token **
> **url.../prousers**

corpo do request (o que vai ser enviado):

```
{
"userId": número do id do usuário /* obrigatório */
"email": "email@email.com", /* regra de negócio */
"name": "nome",  /* regra de negócio */
"phone" : "",  /* regra de negócio - string vazia pois não é pedido no cadastro inicial*/
"bio": "",  /* regra de negócio - string vazia pois não é pedido no cadastro inicial*/
"docs: [],  /* regra de negócio - array vazio pois não é pedido no cadastro inicial*/
bank_info : {} /* regra de negócio - obj vazio pois não é pedido no cadastro inicial*/
}
```

### GET -> Acessa todos os perfils profissionais: 
**precisa de token**
> **url.../prousers**

corpo da resposta (o que vai ser recebido):

```
[
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone, se adicionado via dashboard", 
"bio": "texto sobre o profissional, se adicionado via dashboard", 
"docs: [documentos e outras certificações, se adicionado via dashboard] ,
"bank_info" : {dados bancários para receber da plataforma, se adicionado via dashboard}
},
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone", 
"bio": "texto sobre o profissional", 
"docs: [documentos e outras certificações] ,
"bank_info" : {dados bancários para receber da plataforma}
},
... (array segue com quantos existirem)
]
```

### GET -> Acessa um perfil: 
**precisa de token**
> **url.../prousers/:id**

corpo da resposta (o que vai ser recebido):

```
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone", 
"bio": "texto sobre o profissional", 
"docs: [documentos e outras certificações] ,
"bank_info" : {dados bancários para receber da plataforma}
}
```

### PATCH -> Edita um perfil (será utilizado quando o usuário atualizar o perfil) 
**precisa de token **
> **url.../prousers/:id**

corpo da resposta (o que vai ser enviado):

```
{
"phone": "11111-1111",
"bio": "texto sobre o profissional"
/* apenas os atributos alterados precisam ser enviados, os que não foram alterados não */
}
```

### DELETE -> Exclui um perfil 
**precisa de token**
> **url.../prousers/:id**

Não é necessário enviar nada.


# [/profiles](#Profiles)

Essa rota tem exclusividade de token, ou seja, tudo que for criado nela é acessível apenas para quem tem token, não precisa ser o dono para poder ver, mas precisa ser o dono para editar.
Então será utilizada para mostar o perfil de um usuário para um professor, e deverá ser associada apenas com um usuário do tipo _usuário_

Minha recomendação é que essa associação deve ocorrer no momento do cadastro: form. preenchido com o tipo de conta "usuário" -> requisição na rota /users -> resposta positiva -> utiliza **alguns** dos dados para fazer uma segunda requisição para a essa rota (/profiles). 
Assim o processo já fica automatizado.

## **Métodos:**

### POST -> Cria um novo perfil:
**precisa de token**
> **url.../profiles**

corpo do request (o que vai ser enviado):

```
{
"userId": número do id do usuário /* obrigatório */
"email": "email@email.com", /* regra de negócio */
"name": "nome",  /* regra de negócio */
"phone" : "",  /* regra de negócio - string vazia pois não é pedido no cadastro inicial*/
"bio": "",  /* regra de negócio - string vazia pois não é pedido no cadastro inicial*/
"payment_info" : {}, /* regra de negócio - obj vazio pois não é pedido no cadastro inicial*/
"activities": [], /* regra de negócio - array de atividades iniciado vazio */
"activities_history": [], /* regra de negócio - array de histórico de atividades iniciado vazio */
"activities_favorites": [], /* regra de negócio - array de atividades favoritas iniciado vazio */
}
```

corpo da resposta (o que vai ser recebido):

```
{
"userId": número do id do usuário 
"email": "email@email.com",
"name": "nome",  
"phone" : "", 
"bio": "", 
"payment_info" : {},
"activities": [],
"activities_history": [],
"activities_favorites": [], 
"id": número /* gerado pela api*/
}
```

### GET -> Acessa todos os perfils: 
**precisa de token**
> **url.../profiles**

corpo da resposta (o que vai ser recebido):

```
[
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone, se adicionado via dashboard", 
"bio": "texto sobre o profissional, se adicionado via dashboard", 
"payment_info" : {dados e opções de pagamento, se adicionadas via dashboard}, 
"activities": [lista de atividades atuais, quando agendadas],
"activities_history": [lista de atividades passadas, se existirem], 
"activities_favorites": [lista de atividades favoritas, se existirem],
"id": número
},
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone", 
"bio": "texto sobre o usuário", 
"payment_info" : {dados e opções de pagamento}, 
"activities": [lista de atividades atuais],
"activities_history": [lista de atividades passadas], 
"activities_favorites": [lista de atividades favoritas],
"id": número
},
... (array segue com quantos existirem)
]
```

### GET -> Acessa um perfil: 
**precisa de token**
> **url.../profiles/:id**

corpo da resposta (o que vai ser recebido):

```
{
"userId": número do id do usuário 
"email": "email@email.com", 
"name": "nome",  
"phone" : "telefone", 
"bio": "texto sobre o usuário", 
"payment_info" : {dados e opções de pagamento}, 
"activities": [lista de atividades atuais],
"activities_history": [lista de atividades passadas], 
"activities_favorites": [lista de atividades favoritas],
"id": número
}
```

### PATCH -> Edita um perfil (será utilizado quando o usuário atualizar o perfil) 
**precisa de token**
> **url.../profiles/:id**

corpo da resposta (o que vai ser enviado):

```
{
"phone": "11111-1111",
"bio": "texto sobre o profissional"
/* apenas os atributos alterados precisam ser enviados, os que não foram alterados não */
}
```

### DELETE -> Exclui um perfil 
**precisa de token**
> **url.../profiles/:id**

Não é necessário enviar nada.


# [/activities](#Activities)

Essa rota tem exclusividade de token, ou seja, tudo que for criado nela é acessível para quem tem token, não precisa ser o dono para poder ver, mas precisa ser o dono para editar. Ela será utilizada para criar e acessar as atividades.

## **Métodos:**

### POST -> Cria uma nova atividade:
**precisa de token**
> **url.../activities**

corpo do request (o que vai ser enviado):

```
{
      "userId": id do criador, /*obrigatório */
      "prouserId": id do perfil publico do criador, /* regra de negócio - provavelmente diferente do userId */
      "name": "nome da atividade", /* essa e todo o resto é regra de negócio */
      "type": " tipo de atividade", /* artes marciais, dança, fitness, funcional, wellness, outros */
      "price": número, /* regra de negócio - valor da atividade por participação */
      "users_limit": número limite de pessoas,
      "img_url": "url da imagem",
      "duration_text": "duração em formato texto", /* 1 hora*/
      "duration_ms": numero duração em milisegundos,
      "description": "descrição da atividade",
      ~~"users": [usuários inscritos],~~ /* a api não permite */
      "schedule": {
        "recurrent": boleano se atividade é recorrente,
        "days": [ nome dos dias que as atividades ocorrem ],
        "time_text": "horário de inicio em texto", /*10 h*/
        "start_date": número representando a data /* Date.parse(data)*/
      },
      "address": {
        "line_1": "rua, número e complemento",
        "line_2": " bairro",
        "city": "cidade",
        "state": "estado",
        "zip_code": "cep"
      },
    }
```

corpo da resposta (o que vai ser recebido):

```
{
      "userId": id do criador, 
      "prouserId": id do perfil publico do criador, 
      "name": "nome da atividade", 
      "type": " tipo de atividade", 
      "price": número,
      "users_limit": número limite de pessoas,
      "img_url": "url da imagem",
      "duration_text": "duração em formato texto", 
      "duration_ms": numero duração em milisegundos,
      "description": "descrição da atividade",
      "users": [usuários inscritos],
      "schedule": {
        "recurrent": boleano se atividade é recorrente,
        "days": [ nome dos dias que as atividades ocorrem ],
        "time_text": "horário de inicio em texto", 
        "start_date": número representando a data 
      },
      "address": {
        "line_1": "rua, número e complemento",
        "line_2": " bairro",
        "city": "cidade",
        "state": "estado",
        "zip_code": "cep"
      },
      "id": numero id /* criado pela api */
    }
```

### GET -> Acessa todas as atividades: 
**precisa de token**
> **url.../activities**

corpo da resposta (o que vai ser recebido):

```
[
{
      "userId": id do criador,
      "prouserId": id do perfil publico do criador, 
      "name": "nome da atividade", 
      "type": " tipo de atividade",
      "price": número,
      "users_limit": número limite de pessoas,
      "img_url": "url da imagem",
      "duration_text": "duração em formato texto", 
      "duration_ms": numero duração em milisegundos,
      "description": "descrição da atividade",
      "users": [usuários inscritos],
      "schedule": {
        "recurrent": boleano se atividade é recorrente,
        "days": [ nome dos dias que as atividades ocorrem ],
        "time_text": "horário de inicio em texto", 
        "start_date": número representando a data
      },
      "address": {
        "line_1": "rua, número e complemento",
        "line_2": " bairro",
        "city": "cidade",
        "state": "estado",
        "zip_code": "cep"
      },
      "id": numero id 
 },
{
      "userId": id do criador,
      "prouserId": id do perfil publico do criador, 
      "name": "nome da atividade", 
      "type": " tipo de atividade",
      "price": número,
      "users_limit": número limite de pessoas,
      "img_url": "url da imagem",
      "duration_text": "duração em formato texto", 
      "duration_ms": numero duração em milisegundos,
      "description": "descrição da atividade",
      "users": [usuários inscritos],
      "schedule": {
        "recurrent": boleano se atividade é recorrente,
        "days": [ nome dos dias que as atividades ocorrem ],
        "time_text": "horário de inicio em texto", 
        "start_date": número representando a data
      },
      "address": {
        "line_1": "rua, número e complemento",
        "line_2": " bairro",
        "city": "cidade",
        "state": "estado",
        "zip_code": "cep"
      },
      "id": numero id 
 },
... (array segue com quantas existirem)
]
```

### GET -> Acessa uma atividade: 
**precisa de token**
> **url.../activities/:id**

corpo da resposta (o que vai ser recebido):

```
{
      "userId": id do criador,
      "prouserId": id do perfil publico do criador, 
      "name": "nome da atividade", 
      "type": " tipo de atividade",
      "price": número,
      "users_limit": número limite de pessoas,
      "img_url": "url da imagem",
      "duration_text": "duração em formato texto", 
      "duration_ms": numero duração em milisegundos,
      "description": "descrição da atividade",
      "users": [usuários inscritos],
      "schedule": {
        "recurrent": boleano se atividade é recorrente,
        "days": [ nome dos dias que as atividades ocorrem ],
        "time_text": "horário de inicio em texto", 
        "start_date": número representando a data
      },
      "address": {
        "line_1": "rua, número e complemento",
        "line_2": " bairro",
        "city": "cidade",
        "state": "estado",
        "zip_code": "cep"
      },
      "id": numero id 
 }
 ```

### PATCH -> Edita uma atividade
**precisa de token**
> **url.../activities/:id**

corpo da resposta (o que vai ser enviado):

```
{
"start_date": 111111111,
"description": "texto sobre a atividade alterado"
/* apenas os atributos alterados precisam ser enviados, os que não foram alterados não */
}
```

### DELETE -> Exclui uma atividade 
**precisa de token**
> **url.../activities/:id**

Não é necessário enviar nada.


