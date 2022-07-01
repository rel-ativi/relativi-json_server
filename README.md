# Ballet Class Planner / Exercise Planner </br><sup>Planejador de Auals de Ballet / Exercícios</sup>

This is the backend (json.server Fake API) of the Ballet Class Planner - An application for teachers to plan their classes, and for students to access them.

<sup>Este é o backend (json.server Fake API) do Planejdor de Aulas de Ballet - Uma aplicação para professoras e professores planejarem suas aulas e para estudantes acessa-las.</sup>

- [Register](#Register)
- [Login](#Login)
- [User](#User)
- [Classes](#Classes)
- [Exercises](#Exercises)
- [Sequences](#Sequences)

## Endpoints
As stated in [JSON-Server-Auth Documentation](https://www.npmjs.com/package/json-server-auth), there are 3 endpoints that can be used to create a new user and 2 endpoints for login.
<sup>Assim como a [documentação do JSON-Server-Auth](https://www.npmjs.com/package/json-server-auth), existem 3 endpoints que podem ser utilizados para cadastro e 2 endpoints que podem ser usados para login.</sup>

<dl id="Register">
<dt><h3><a href="#Register"><b>Register</b></a><sub>|Cadastro</sub></h3></dt>
<dd><code>POST - /register</code></dd>
<dd><code>POST - /signup</code></dd>
<dd><code>POST - /users</code></dd>
</dl>
Any of this 3 endpoints will create a new user. The required fields are email and password. For this application is recommended the addition of a type attribute (teacher or student) and of an empty array of teachers if the type is student and of students if the type is teacher.
</br></br>
<sup>pt-BR</br>Qualquer um desses 3 endpoints irá cadastrar o usuário na lista de "Users", sendo que os campos obrigatórios são os de email e password.
Para essa aplicação é recomendado a adição de um tipo (teacher ou student) e de um array vazio de teachers se o tipo for student e de students se o tipo for teacher.</sup>


<dl id="Login">
<dt><h3><a href="#Login"><b>Login</b></a></h3></dt>
<dd><code>POST - /login</code><dd/>
<dd><code>POST - /signin</code><dd>
</dl>
Any of these 2 endpoints can be used to login with an existent user. All the following endpoints require a token to be used.
</br></br>
<sup>pt-BR</br>Qualquer um desses 2 endpoints pode ser usado para realizar login com um dos usuários cadastrados na lista de "Users". Todos os endpoints a seguir precisam de um token para serem utilizados.</sup>
<dl>
</dl>

<dl id="User">
<dt><h3><a href="#User"><b>User</b></a><sub>| Usuário</sub></h3></dt>
<dd><code>GET - /users/:id</code></dd>
<dd><code>PUT - /users/:id</code></dd>
<dd><code>PATCH - /users/:id</code></dd>
<dd><code>DELETE - /users/:id</code></dd>
</dl>
Use these endpoints to get (GET) details about an user, update (PUT, PATCH) and user, or remove (DELETE) an user from the database. 

**requires token**
</br></br>
<sup>pt-BR</br>Utilizer esses endpoints para acessar (GET) os detalhes de um usuário, atualizar (PUT, PATCH) um usuário, ou remover (DELETE) um usuário do banco de dados.</br><b>token necessário</b></sup>

<dl id="Classes">
<dt><h3><a href="#Classes"><b>Classes</b></a><sub>| Aulas</sub></h3></dt>
<dd><code>POST - /classes</code></dd>
<dd><code>PUT - /classes/:id</code></dd>
<dd><code>PATCH - /classes/:id</code></dd>
<dd><code>DELETE - /classes/:id</code></dd>
</dl>

Write permissions (PUT, PATCH, DELETE) only available for owner's token, the owner id must be passed at the moment of creation (POST). Can be seen by all registered users, any access restrictions must be performed on the frontend.

**requires token**
</br></br>
<sup>pt-BR</br>Permissões de edição (PUT, PATCH, DELETE) disponível apenas para o token do proprietário o id do proprietário deve ser passado no momento da criação (POST). Podem ser vizualizadas por qualquer usuário logado, restrições de visualização devem ser realizadas no frontend</br><b>token necessário</b></sup>

### Creating a new class <sub>| criando uma nova aula</sub>
```POST /classes - Body Content```

<sup>Conteúdo do body</sup>
<details>

```json
{
      "name": "Ballet Class 1st Year",
      "userId": 1
}
```
</details>
</br>

```POST /classes - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
	"name": "Ballet Class 1st Year",
	"userId": 1,
	"id": 1
}
```
</details>
</br>

```POST /classes - Response Format - 403 Forbidden (No user ID)```

<sup>Formato da resposta sem ID do usuário</sup>
<details>

```json
{
    "Private resource creation: request body must have a reference to the owner id"
}
```
</details>
</br>

### Getting classes <sub>| Listar aulas</sub>
#### **All classes** <sub>Todas as aulas</sub>

```GET /classes - Response Format - 200 Ok```

<sup>Formato da resposta</sup>

<details>

```json
[
	{
		"name": "Ballet Class 1st Year",
		"userId": 1,
		"id": 1
	}
]
```
</details>  
</br>

#### **All user classes** <sub>Todas as aulas de um usuário</sub>  
```GET users/:userId/classes - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Ballet Class 1st Year",
		"userId": 1,
		"id": 1
	}
]
```
</details>
</br>

#### **Classe by id** <sub>Aulas por Id</sub>  
```GET /classes/:classId - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
    "name": "Ballet Class 1st Year",
    "userId": 1,
    "id": 1
}
```
</details>
</br>

#### **Classes by name** <sub>Aulas por nome</sub>  
```GET /classes?name=Ballet Class 1st Year - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Ballet Class 1st Year",
		"userId": 1,
		"id": 1
	}
]
```
</details>
</br>

#### **Class + Exercises** <sub>Aula + Exercícios</sub>  
```GET /classes/:classId?_embed=exercises - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
	"name": "Ballet Class 1st Year",
	"userId": 1,
	"id": 1,
	"exercises": [
		{
			"name": "Pliés",
			"classId": 1,
			"userId": 1,
			"id": 1
		},
		{
			"name": "Tandu",
			"classId": 1,
			"userId": 1,
			"id": 2
		},
		{
			"name": "Glissé",
			"classId": 1,
			"userId": 1,
			"id": 3
		},
		{
			"name": "Jeté",
			"classId": 1,
			"userId": 1,
			"id": 4
		}
	]
}
```
</details>
<dl id="Exercises">
<dt><h3><a href="#Exercises"><b>Exercises</b></a><sub>| Exercícios</sub></h3></dt>
<dd><code>POST - /exercises</code></dd>
<dd><code>PUT - /exercises/:id</code></dd>
<dd><code>PATCH - /exercises/:id</code></dd>
<dd><code>DELETE - /exercises/:id</code></dd>
</dl>

Write permissions (PUT, PATCH, DELETE) only available for owner's token, the owner id along with the class id must be passed at the moment of creation (POST). Can be seen by all registered users, any access restrictions must be performed on the frontend.

**requires token**
</br></br>
<sup>pt-BR</br>Permissões de edição (PUT, PATCH, DELETE) disponível apenas para o token do proprietário o id do proprietário junto com o id da aula deve ser passado no momento da criação (POST). Podem ser vizualizadas por qualquer usuário logado, restrições de visualização devem ser realizadas no frontend</br><b>token necessário</b></sup>

### Creating a new exercise <sub>| criando um novo execício</sub>
```POST /exercises - Body Content```

<sup>Conteúdo do body</sup>
<details>

```json
{
  "name": "Pliés",
  "classId": 1,    
  "userId": 1
}
```
</details>
</br>

```POST /exercises - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
  "name": "Pliés",
  "classId": 1,    
  "userId": 1,
  "id":1,
}
```
</details>
</br>

```POST /exercises - Response Format - 403 Forbidden (No user ID)```

<sup>Formato da resposta sem ID do usuário</sup>
<details>

```json
{
    "Private resource creation: request body must have a reference to the owner id"
}
```
</details>

### Getting exercises <sub>| Listar exercícios</sub>
#### **All exercises** <sub>Todos exercícios</sub>  
```GET /exercises - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Pliés",
		"classId": 1,
		"userId": 1,
		"id": 1
	},
	{
		"name": "Tandu",
		"classId": 1,
		"userId": 1,
		"id": 2
	},
	{
		"name": "Glissé",
		"classId": 1,
		"userId": 1,
		"id": 3
	},
	{
		"name": "Jeté",
		"classId": 1,
		"userId": 1,
		"id": 4
	}
]
```
</details>
</br>

#### **All exercises in a class** <sub>Todos exercícios em uma aula</sub>  
```GET classes/:classId/exercises - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Pliés",
		"classId": 1,
		"userId": 1,
		"id": 1
	},
	{
		"name": "Tandu",
		"classId": 1,
		"userId": 1,
		"id": 2
	},
	{
		"name": "Glissé",
		"classId": 1,
		"userId": 1,
		"id": 3
	},
	{
		"name": "Jeté",
		"classId": 1,
		"userId": 1,
		"id": 4
	}
]
```
</details>
</br>

##### **Exercise by id** <sub>Exercício por Id</sub>  
```GET /exercises/:exerciseId - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
	"name": "Pliés",
	"classId": 1,
	"userId": 1,
	"id": 1
}
```
</details>
</br>

#### **Exercises by name** <sub>Exercícios por nome</sub>  
```GET /exercises?name=Tendu - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Tandu",
		"classId": 1,
		"userId": 1,
		"id": 2
	}
]
```
</details>
</br>

#### **Exercises + Sequences** <sub>Exercícios + Sequências</sub>  
```GET /exercises?_embed=sequences - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"name": "Pliés",
		"classId": 1,
		"userId": 1,
		"id": 1,
		"sequences": [
			{
				"sequence": [
					"1ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse: 8t",
					"2ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse Barra: 8t",
					"4ª - 2x Grand: 16t, Suplesse Out: 8t",
					"5ª - 2x Grand: 16t, Cambret: 8t",
					"Sous-sus, detourne, talon, degagé, 1ª: 6t",
					"1ª - 4x Elevé: 8t, 4x Demi (demi-pointe): 8t, Balance"
				],
				"music": {
					"name": "Lucy",
					"album": "60s",
					"artist": "Andrew Holdsworth",
					"link": ""
				},
				"exerciseId": 1,
				"classId": 1,
				"userId": 1,
				"id": 1
			}
		]
	},
	{
		"name": "Tandu",
		"classId": 1,
		"userId": 1,
		"id": 2,
		"sequences": [
			{
				"sequence": [
					"1ª - En Croix en dehors: 16t, 1x: 2t, 2x: 1t",
					"Repeat en dedans",
					"1ª - Em Croix em dehors: 16t, Tandu, foundue, fermé, alongé: 4t",
					"Repeat en dedans"
				],
				"music": {
					"name": "Grape Vine",
					"album": "60s",
					"artist": "Andrew Holdsworth",
					"link": ""
				},
				"exerciseId": 2,
				"classId": 1,
				"userId": 1,
				"id": 2
			}
		]
	},
	{
		"name": "Glissé",
		"classId": 1,
		"userId": 1,
		"id": 3,
		"sequences": [
			{
				"sequence": [
					"1ª - En croix en dehors: 16t, 1x: 4t",
					"Repeat en dedans",
					"1ª - En croix en dehors: 16t 2x: 4t",
					"Sous-sus, detourne, talon, degagé, 1ª",
					"repeat other side"
				],
				"music": {
					"name": "Eileen",
					"album": "60s",
					"artist": "Andrew Holdsworth",
					"link": ""
				},
				"exerciseId": 3,
				"classId": 1,
				"userId": 1,
				"id": 3
			}
		]
	},
	{
		"name": "Jeté",
		"classId": 1,
		"userId": 1,
		"id": 4,
		"sequences": [
			{
				"sequence": [
					"1ª - En croix en dehors: 16t, 1x: 4t - hold-out: 3t",
					"Repeat en dedans",
					"1ª - Em croix em dehors: 32t:",
					"En Croix en dehors: 1x (4t) - hold out",
					"En Croix en dedans: 1x (4t) - hold out",
					"En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
					"En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
					"sous-sus / detourne"
				],
				"music": {
					"name": "Madonna",
					"album": "60s",
					"artist": "Andrew Holdsworth",
					"link": ""
				},
				"exerciseId": 4,
				"classId": 1,
				"userId": 1,
				"id": 4
			}
		]
	}
]
```
</details>
<dl id="Sequences">
<dt><h3><a href="#Sequences"><b>Sequences</b></a><sub>| Sequências</sub></h3></dt>
<dd><code>POST - /sequences</code></dd>
<dd><code>PUT - /sequences/:id</code></dd>
<dd><code>PATCH - /sequences/:id</code></dd>
<dd><code>DELETE - /sequences/:id</code></dd>
</dl>

Write permissions (PUT, PATCH, DELETE) only available for owner's token, the owner id along with the class id and exercise id must be passed at the moment of creation (POST). Can be seen by all registered users, any access restrictions must be performed on the frontend.

**requires token**
</br></br>
<sup>pt-BR</br>Permissões de edição (PUT, PATCH, DELETE) disponível apenas para o token do proprietário o id do proprietário junto com o id da aula e o id do exercício deve ser passado no momento da criação (POST). Podem ser vizualizadas por qualquer usuário logado, restrições de visualização devem ser realizadas no frontend</br><b>token necessário</b></sup>

### Creating a new sequence <sub>| criando uma nova sequência</sub>
```POST /sequences - Body Content```

<sup>Conteúdo do body</sup>
<details>

```json
{
    "sequence": [
        "1ª - En croix en dehors: 16t, 1x: 4t - hold-out: 3t",
        "Repeat en dedans",
        "1ª - Em croix em dehors: 32t:",
        "En Croix en dehors: 1x (4t) - hold out",
        "En Croix en dedans: 1x (4t) - hold out",
        "En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
        "En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
        "sous-sus / detourne"
    ],
    "music": {
        "name": "Madonna",
        "album": "60s",
        "artist": "Andrew Holdsworth",
        "link": "" //optional
    },
    "exerciseId": 4,
    "classId": 1,
    "userId": 1,
}
```
</details>

```POST /exercises - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
    "sequence": [
        "1ª - En croix en dehors: 16t, 1x: 4t - hold-out: 3t",
        "Repeat en dedans",
        "1ª - Em croix em dehors: 32t:",
        "En Croix en dehors: 1x (4t) - hold out",
        "En Croix en dedans: 1x (4t) - hold out",
        "En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
        "En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
        "sous-sus / detourne"
    ],
    "music": {
        "name": "Madonna",
        "album": "60s",
        "artist": "Andrew Holdsworth",
        "link": "" //optional
    },
    "exerciseId": 4,
    "classId": 1,
    "userId": 1,
    "id": 4,
}
```
</details>

```POST /exercises - Response Format - 403 Forbidden (No user ID)```

<sup>Formato da resposta sem ID do usuário</sup>
<details>

```json
{
    "Private resource creation: request body must have a reference to the owner id"
}
```
</details>
</br>

### Getting sequences <sub>| Listar seqências</sub>
**All sequences** <sub>Todos exercícios</sub>  
```GET /sequences - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"sequence": [
			"1ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse: 8t",
			"2ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse Barra: 8t",
			"4ª - 2x Grand: 16t, Suplesse Out: 8t",
			"5ª - 2x Grand: 16t, Cambret: 8t",
			"Sous-sus, detourne, talon, degagé, 1ª: 6t",
			"1ª - 4x Elevé: 8t, 4x Demi (demi-pointe): 8t, Balance"
		],
		"music": {
			"name": "Lucy",
			"album": "60s",
			"artist": "Andrew Holdsworth",
			"link": ""
		},
		"exerciseId": 1,
		"classId": 1,
		"userId": 1,
		"id": 1
	},
	{
		"sequence": [
			"1ª - En Croix en dehors: 16t, 1x: 2t, 2x: 1t",
			"Repeat en dedans",
			"1ª - Em Croix em dehors: 16t, Tandu, foundue, fermé, alongé: 4t",
			"Repeat en dedans"
		],
		"music": {
			"name": "Grape Vine",
			"album": "60s",
			"artist": "Andrew Holdsworth",
			"link": ""
		},
		"exerciseId": 2,
		"classId": 1,
		"userId": 1,
		"id": 2
	},
	{
		"sequence": [
			"1ª - En croix en dehors: 16t, 1x: 4t",
			"Repeat en dedans",
			"1ª - En croix en dehors: 16t 2x: 4t",
			"Sous-sus, detourne, talon, degagé, 1ª",
			"repeat other side"
		],
		"music": {
			"name": "Eileen",
			"album": "60s",
			"artist": "Andrew Holdsworth",
			"link": ""
		},
		"exerciseId": 3,
		"classId": 1,
		"userId": 1,
		"id": 3
	},
	{
		"sequence": [
			"1ª - En croix en dehors: 16t, 1x: 4t - hold-out: 3t",
			"Repeat en dedans",
			"1ª - Em croix em dehors: 32t:",
			"En Croix en dehors: 1x (4t) - hold out",
			"En Croix en dedans: 1x (4t) - hold out",
			"En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
			"En Croix en dehors: 1x (4t) - hold out / 1x (1t) - wait (3t)",
			"sous-sus / detourne"
		],
		"music": {
			"name": "Madonna",
			"album": "60s",
			"artist": "Andrew Holdsworth",
			"link": ""
		},
		"exerciseId": 4,
		"classId": 1,
		"userId": 1,
		"id": 4
	}
]
```
</details>
</br>

#### **All sequences in an exercise** <sub>Todas sequências em um exercícios</sub>  

```GET exercises/:exerciseId/sequences - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
[
	{
		"sequence": [
			"1ª - En Croix en dehors: 16t, 1x: 2t, 2x: 1t",
			"Repeat en dedans",
			"1ª - Em Croix em dehors: 16t, Tandu, foundue, fermé, alongé: 4t",
			"Repeat en dedans"
		],
		"music": {
			"name": "Grape Vine",
			"album": "60s",
			"artist": "Andrew Holdsworth",
			"link": ""
		},
		"exerciseId": 2,
		"classId": 1,
		"userId": 1,
		"id": 2
	}
]
```
</details>
</br>

#### **Sequence by id** <sub>Sequência por Id</sub>  

```GET /sequences/:sequenceId - Response Format - 200 Ok```

<sup>Formato da resposta</sup>
<details>

```json
{
	"sequence": [
		"1ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse: 8t",
		"2ª - 2x Demi: 8t, 1x Grand: 8t, 1x Suplesse Barra: 8t",
		"4ª - 2x Grand: 16t, Suplesse Out: 8t",
		"5ª - 2x Grand: 16t, Cambret: 8t",
		"Sous-sus, detourne, talon, degagé, 1ª: 6t",
		"1ª - 4x Elevé: 8t, 4x Demi (demi-pointe): 8t, Balance"
	],
	"music": {
		"name": "Lucy",
		"album": "60s",
		"artist": "Andrew Holdsworth",
		"link": ""
	},
	"exerciseId": 1,
	"classId": 1,
	"userId": 1,
	"id": 1
}
```
</details>
