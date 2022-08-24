# Challenge


## Descripción del problema

En **Mercado Libre** trabajamos con `articulos` de sellers que los venden a traves de nuestro marketplace, y sobre `usuarios` que tienen interés en comprar esos `articulos`. El objetivo de este desafío es realizar una aplicación la cual exponga un _API_ que permita realizar algunas operaciones de _CRUD_ para cada una de esas dos entidades con algunas reglas de negocio sobre ellas.


### Articulos

Un item, tiene la información básica sobre el artículo que será anunciado por nosotros. 

#### Representación de un item

A continuación un ejemplo de la representación _JSON_ de un item:

```json
{
  "id": 10,
  "code": "SAM27324354",
  "title": "Tablet Samsung Galaxy Tab S7",
  "description": "Galaxy Tab S7 with S Pen SM-t733 12.4 pulgadas y 4GB de memoria RAM",
  "price": 150000,
  "stock": 3,
  "photos": [
    "https://http2.mlstatic.com/D_NQ_NP_729539-MLA48049063325_102021-O.jpg",
    "https://http2.mlstatic.com/D_NQ_NP_879745-MLA48049070326_102021-O.jpg"
  ],
  "itemType": "SELLER",
  "leader": true,
  "leaderLevel": "PLATINUM",
  "createdAt": "2020-05-10T04:20:33Z",
  "updatedAt": "2020-05-10T05:30:00Z",
  "status": "ACTIVE"
}
```

#### Reglas sobre artículos

1. Los _ids_ deben ser numéricos generados automáticamente a partir del número 1.
2. Los campos `code`, `title`, `description`, `price`, `stock`, `photos` son obligatórios.
3. El campo `code` debe ser único.
4. Los campos `createdAt`, `updatedAt` son automáticamente generados por el sistema. La API no debería permitir modificaciones sobre ellos.
5. El campo `itemType` puede tener los valores {`OWN`, `SELLER`}

Para propósito del ejercicio vamos a tener algunas reglas sobre si un item es válido o no dependiendo su tipo.

6. Caso en que el item es de un vendedor: `"itemType": "SELLER"`
	
    6.1. El campo `leader` tiene como valor true o false.
    6.2. Si el campo `leader` tiene como valor true, el campo `leaderLevel` debe tener uno de los siguientes valores:  `BASIC`, `GOLD` o `PLATINUM`.

7. Caso en que el item es propio: `"itemType": "OWN"`
   
	7.1. El campo `leader` no es obligatorio (puede guardar false por defecto).
	7.2. El campo `leaderLevel` debe estar vacío.

8. El campo `status` puede tener los siguientes valores: 
 - `ACTIVE`: Un item que cumple con todas las reglas anteriormente descritas y hay stock disponible.
 - `INACTIVE`: Un item que cumple con todas las reglas anteriormente descritas y pero el valor de stock es cero (0). 


#### Desafío

Usando las siguientes estructuras de `Request`, `Body` y `Response` permita las siguientes funcionalidades. Para crear o editar items, tenga en cuenta las reglas descritas anteriormente.


1. **Cree nuevos items.**

_Request_:
```
POST v1/items
```
_Body_:

```json
{
  "code": "SAM27324354",
  "title": "Tablet Samsung Galaxy Tab S7",
  "description": "Galaxy Tab S7 with S Pen SM-t733 12.4 pulgadas y 4GB de memoria RAM",
  "price": 150000,
  "stock": 15,
  "photos": [
    "https://http2.mlstatic.com/D_NQ_NP_729539-MLA48049063325_102021-O.jpg",
    "https://http2.mlstatic.com/D_NQ_NP_879745-MLA48049070326_102021-O.jpg"
  ],
  "itemType": "SELLER",
  "leader": true,
  "leaderLevel": "PLATINUM"
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

2. **Actualice un item**
 
_Request_:

```
PUT v1/items/{id}
```

_Body_:

```json
{
  "code": "SAM27324354",
  "title": "Tablet Samsung Galaxy Tab S7",
  "description": "Galaxy Tab S7 with S Pen SM-t733 12.4 pulgadas y 4GB de memoria RAM",
  "price": 158000,
  "stock": 25,
  "photos": [
    "https://http2.mlstatic.com/D_NQ_NP_729539-MLA48049063325_102021-O.jpg",
    "https://http2.mlstatic.com/D_NQ_NP_879745-MLA48049070326_102021-O.jpg",
        "https://http2.mlstatic.com/D_NQ_NP_2X_849503-MLA48049083262_102021-F.jpg"
  ],
  "itemType": "SELLER",
  "leader": true,
  "leaderLevel": "PLATINUM"
}
```

_Response_:
  
Usted define la respuesta, hace parte del desafío

3. **Obtener todos los items filtrados por _Status_**

Retorna los items filtrados por _Status_ (opcional). Los resultados vienen paginados y organizados por fecha de actualización del más reciente al más antiguo. Si no tiene fecha de actualización, debe usar la fecha de creación para ordenar.

_Request_:
  
```
GET v1/items?status={status}&page={pageNumber}&pageSize={pageSize}
```

Donde:

- `status`: Es el filtro por el estado del item; es un parámetro opcional y su valor default es `ALL`. Puede tener los siguientes valores:
	- `ALL`: Retorna todos los items sin importar su valor en el campo el estado
	- `ACTIVE`: Retorne los items activos
	- `INACTIVE`: Retorne los items inactivos
- `page`: Es el número de la página; es un parámetro opcional y su valor default es 1
- `pageSize`: Es el tamaño solicitado de resultados en la página. Es un parámetro opcional, su valor default es 10, y su valor máximo es 20.


_Response_:

La respuesta debe seguir la siguiente estructura de campos:

- `page`: La página actual de los resultados
- `pageSize`: El máximo número de resultados retornado por página
- `total`: El número total de items
- `totalPages`: El número total de items que contienen resultados para la búsqueda hecha.
- `data`: Un array con los objetos conteniendo los items solicitados en el request.

```json
{
  "page": 1,
  "pageSize": 10,
  "totalPages": 1,
  "total": 1,
  "data": [
    {
    "id": 10,
    "code": "SAM27324354",
    "title": "Tablet Samsung Galaxy Tab S7",
    "description": "Galaxy Tab S7 with S Pen SM-t733 12.4 pulgadas y 4GB de memoria RAM",
    "price": 150000,
    "stock": 3,
    "photos": [
        "https://http2.mlstatic.com/D_NQ_NP_729539-MLA48049063325_102021-O.jpg",
        "https://http2.mlstatic.com/D_NQ_NP_879745-MLA48049070326_102021-O.jpg"
    ],
    "itemType": "SELLER",
    "leader": true,
    "leaderLevel": "PLATINUM",
    "createdAt": "2020-05-10T04:20:33Z",
    "updatedAt": "2020-05-10T05:30:00Z",
    "status": "ACTIVE"
    }
  ]
}
```

### Usuarios

Tenemos usuarios que guardan como favoritos algunos de los items. Para propósito de este ejercicio, la única información con la que vamos a contar sobre el usuario es su email. Y los usuarios pueden tener algunos items marcados como favoritos.

Solo si el usuario está autenticado puede marcar como favoritos sus artículos, y listar sus artículos favoritas. 

Queda a su criterio escoger una forma para autenticar el usuario.

#### Desafío

Usando las siguientes estructuras de `Request`, `Body` y `Response` permita las siguientes funcionalidades. Para crear o editar artículos, tenga en cuenta las reglas descritas anteriormente.


1. **Cree un usuario**
 
_Request_:

```
POST v1/users/
```

_Body_:

```json
{
  "email": "test_user@mercadolibre.com",
  "password": "test"
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

2. **Autentique un usuario**

_Request_:

```
POST v1/users/login
```

_Body_:
```json
{
  "email": "test_user@mercadolibre.com",
  "password": "test"
}
```

_Response_:

Usted define la respuesta, hace parte del desafío


3. **Marque como favorito un item de un usuario autenticado**

_Request_:

Adapte su *request* para enviar los datos de autenticación del usuario

```
POST v1/users/me/favorites
```

_Body_:
```json
{
  "itemId": 52
}
```

_Response_:

Usted define la respuesta, hace parte del desafío

4. **Retorna los items favoritos del usuario autenticado**

Para este ejercicio solo retorne los items favoritos del usuario que están activas.

_Request_:

Adapte su *request* para enviar los datos de autenticación del usuario

```
GET v1/users/me/favorites
```

_Response_:
```json
{
  "page": 1,
  "pageSize": 10,
  "totalPages": 1,
  "total": 1,
  "data": [
    {
    "id": 10,
    "code": "SAM27324354",
    "title": "Tablet Samsung Galaxy Tab S7",
    "description": "Galaxy Tab S7 with S Pen SM-t733 12.4 pulgadas y 4GB de memoria RAM",
    "price": 150000,
    "stock": 3,
    "photos": [
      "https://http2.mlstatic.com/D_NQ_NP_729539-MLA48049063325_102021-O.jpg",
      "https://http2.mlstatic.com/D_NQ_NP_879745-MLA48049070326_102021-O.jpg"
    ],
    "itemType": "SELLER",
    "leader": true,
    "leaderLevel": "PLATINUM",
    "createdAt": "2020-05-10T04:20:33Z",
    "updatedAt": "2020-05-10T05:30:00Z",
    "status": "ACTIVE"
    }
  ]
}
```

## Criterios de calificación

Esperamos que el código que usted va a crear sea considerado por usted como _"Production Ready"_; por favor use las buenas prácticas a las cuáles usted está acostumbrado en su rutina de desarrollo de código.

Para la evaluación de su código, esperamos que su código sea portable. Esperamos que usted nos provea un comando para correr fácilmente en el ambiente local, la solución del problema.

Para el desarrollo del desafío usted puede utilizar alguna de las siguientes lenguajes de programación:

- Golang, 
- Java

Dentro de los criterios que vamos a tener en cuenta a la hora de revisar su código, revisaremos:

- Resuelve el problema propuesto
- Organización y estructura del proyecto
- Mantenibilidad
- Facilidad para hacer tests
- Valoraremos adicionalmente si usa alguna arquitectura limpia.
