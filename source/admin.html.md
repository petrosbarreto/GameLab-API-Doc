---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  # - ruby
  # - python
  # - javascript

toc_footers:
  - <a href='https://www.instabuy.com.br'>Build with Love by Instabuy</a>

includes:
  # - kittens
  # - errors

search: true
---

# Introduction

Welcome to GameLab's REST **Admin** API.

To check other docs, click above:

- [Client Doc](/).

## URLs

For development purpose you should use [http://api.gamelab.com.br/api_admin_v1](http://api.gamelab.com.br/api_admin_v1).


## Request/Response Format

The default response format is **JSON**. Requests with a message-body use plain JSON to set or update resource attributes. Successful requests will return a `200 OK` HTTP status.

Some general information about responses:

- Dates are returned in ISO8601 format: `YYYY-MM-DDTHH:MM:SS`
- Any decimal monetary amount, such as prices or totals, will be returned as numbers with two decimal places.
- Other amounts, such as item counts, are returned as integers.
- Blank/Null fields are generally omitted.

### Response Format

All server's response have 3 fields in it: `data`, `count`, `http_status`.

#### Data

The info itself. Here it could be an error message, if status is error, or list, object, string, etc if status is success.

#### Count

The number of objects the query should return if no pagination/filter were applied.

#### HTTP Status

The response's HTTP Status.

<!-- ## Errors
Occasionally you might encounter errors when accessing the REST API. Below are the possible types:

Error Code | Error Type
-------------- | --------------
`211 Unauthorized` | You are trying to access a protected endpoint but dont have permission / aren't logged.
`212 User Blocked` | You were blocked from the system.
`213 Not Found` | Query didn't return results.
`214 Expected Error` | Expected Error, e.g.: system could not save item because slug is not unique.
`218 Reload Cart` | One or more items on the user's cart had stock or price changed, please get cart again.
`400 Bad Request` | Invalid request, e.g. using an unsupported HTTP method
`404 Not Found` | Requests to resources that don't exist or are missing
`405 Method Not Allowed` | You tried to access a endpoint with an invalid method.
`500 Internal Server Error` | Server error -->


## Pagination
Requests that return multiple items will be paginated to 25 items by default. This default can be changed specifing the `?N` parameter:

`GET /buys?N=15`

You can specify further pages with the `?page` parameter:

`GET /buys?page=2`

You can use both parameters as well:

`GET /buys?page=5&N=15`

Page number is `1-based` and omitting the `?page` parameter will return the first page.

The total number of resources are included in the response `count` field.

## Images
All fields that represent images have only the image Identifier and not the image URL.
<!-- todo -->

# Authentication

The Authentication in GameLab's API is made using **COOKIES**. Therefore, to log in and access protected endpoints you must have cookies enabled on your browser/app.

# Answer
This API allow you to CRUD answers.

## Answer properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`title` | string | Answer title.
`next_question` | Object | If user select this answer, what question should be the next. See [Question](#question-properties).
`finish_module` | Object | If user select this answer, what module should appear next. See [Module](#module-properties).

Answer can have either `next_question` or `finish_module`.

## Create Answer
```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/answer \
  -H 'cookie: GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8' \
  -F 'title=Não' \
  -F question_id=5cf968ff54d723678ade3adf
```

> JSON response example:

```json
{
    "data": {
        "created_at": "2019-06-06T19:37:01.286000+00:00",
        "title": "Não",
        "last_modified": "2019-06-06T19:37:01.286000+00:00",
        "id": "5cf96b5d1ed86d47bc64c75b",
        "creator": "cayke@instabuy.com.br"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /answer`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`title` | string | required | Answer title.
`question_id` | string | required | Question where answer should be added.


# Demand
This API allow you to CRUD city demands.

## Demand properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`state` | string | State.
`city` | string | City.
`demands` | array | List of demands.

## Get Demands
```shell
curl -X GET \
  http://api.gamelab.tk/api_admin_v1/demand \
  -H 'cookie: GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8' \
```

> JSON response example:

```json
{
    "data": [
        {
            "city": "Brasilia",
            "state": "DF",
            "last_modified": "2019-06-27T18:10:41.273000+00:00",
            "id": "5d128fe1c7f924a22c3a8620",
            "demands": [
                "Concurso Publico",
                "Politica"
            ],
            "created_at": "2019-06-25T21:19:29.552000+00:00",
            "creator": "cayke@instabuy.com.br"
        }
    ],
    "count": 1,
    "http_status": 200
}
```

### HTTP Request
`GET /demand`

## Create City
```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/demand \
  -H 'cookie: GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8' \
  -F 'city=Brasília' \
  -F 'state=DF' \
```

> JSON response example:

```json
{
    "data": {
        "created_at": "2019-06-25T21:19:29.552000+00:00",
        "demands": [],
        "state": "DF",
        "id": "5d128fe1c7f924a22c3a8620",
        "creator": "cayke@instabuy.com.br",
        "last_modified": "2019-06-25T21:19:29.552000+00:00",
        "city": "Brasilia"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /demand`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`city` | string | required | City.
`state` | string | required | State.


## Add Demand to city
```shell
curl -X PUT \
  http://api.gamelab.tk/api_admin_v1/demand \
  -H 'cookie: GLAdminSessionCookie=09eb5c3f2f0a4349a607f183b939cab1' \
  -F id=5d128fe1c7f924a22c3a8620 \
  -F 'demand=Concurso Publico'
```

> JSON response example:

```json
{
    "data": {
        "city": "Brasilia",
        "state": "DF",
        "last_modified": "2019-06-25T21:19:29.552000+00:00",
        "id": "5d128fe1c7f924a22c3a8620",
        "demands": [
            "Concurso Publico"
        ],
        "created_at": "2019-06-25T21:19:29.552000+00:00",
        "creator": "cayke@instabuy.com.br"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`PUT /demand`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`id` | string | required | City id.
`demand` | string | required | Demand to be added.

## Remove Demand from city
```shell
curl -X DELETE \
  http://api.gamelab.tk/api_admin_v1/demand \
  -H 'cookie: GLAdminSessionCookie=09eb5c3f2f0a4349a607f183b939cab1' \
  -F id=5d128fe1c7f924a22c3a8620 \
  -F 'demand=Concurso Publico'
```

> JSON response example:

```json
{
    "data": {
        "city": "Brasilia",
        "state": "DF",
        "last_modified": "2019-06-27T18:10:41.273000+00:00",
        "id": "5d128fe1c7f924a22c3a8620",
        "demands": [
            "Politica"
        ],
        "created_at": "2019-06-25T21:19:29.552000+00:00",
        "creator": "cayke@instabuy.com.br"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`DELETE /demand`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`id` | string | required | City id.
`demand` | string | required | Demand to be added.


## Remove City
```shell
curl -X DELETE \
  http://api.gamelab.tk/api_admin_v1/demand \
  -H 'cookie: GLAdminSessionCookie=09eb5c3f2f0a4349a607f183b939cab1' \
  -F id=5d128fe1c7f924a22c3a8620
```

> JSON response example:

```json
{
    "data": "ok",
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`DELETE /demand`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`id` | string | required | City id.


# Login
This API allow you to login user. You must pass `email` and `password`.

```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/login \
  -F email=cayke10@gmail.com \
  -F password=123456
```

> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /login`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | optional | User email.
`password` | string | optional | User password.


# Logout
This API allow you to logout user.

```shell
curl -X DELETE \
  http://api.gamelab.tk/api_admin_v1/logout \
  -H 'cookie: GLAdminSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
```

> JSON response example:

```json
{  
   "data":"ok",
   "count":0,
   "http_status":200
}
```

### HTTP Request
`DELETE /logout`

# Module
This API allows you to CRUD modules.

## Module properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`title` | string | Module title.
`image` | string | Module image key.
`description` | string | Module description.

## Create Module
```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/module \
  -H 'cookie: GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8;' \
  -F 'title=Modulo Curiosidade' \
  -F 'description=Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ultrices varius nisl, vitae volutpat lorem ultricies in. Nunc accumsan porta nulla, id maximus justo. Aliquam erat volutpat. Mauris in nisi scelerisque, accumsan libero in, posuere ante. Nam euismod efficitur feugiat. Maecenas vestibulum, mi quis dapibus semper, nisl lorem varius metus, bibendum convallis dui risus viverra tellus. Nam placerat hendrerit quam vel suscipit. Mauris sagittis lectus ac risus ullamcorper suscipit. Suspendisse et erat id odio mollis volutpat non quis ante. Vestibulum posuere commodo augue, ut mattis turpis bibendum nec. Mauris scelerisque, est ac tempus consectetur, risus tortor facilisis ante, non faucibus felis ex at nibh. Vivamus interdum, mauris in tincidunt venenatis, elit purus tempus diam, sit amet euismod ligula massa vel nibh. Donec vulputate consequat tempus. Curabitur id vulputate nibh.' \
  -F answer_id=5cf96b5d1ed86d47bc64c75b
```

> JSON response example:

```json
{
    "data": {
        "title": "Modulo Curiosidade",
        "created_at": "2019-06-06T20:39:16.818000+00:00",
        "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ultrices varius nisl, vitae volutpat lorem ultricies in. Nunc accumsan porta nulla, id maximus justo. Aliquam erat volutpat. Mauris in nisi scelerisque, accumsan libero in, posuere ante. Nam euismod efficitur feugiat. Maecenas vestibulum, mi quis dapibus semper, nisl lorem varius metus, bibendum convallis dui risus viverra tellus. Nam placerat hendrerit quam vel suscipit. Mauris sagittis lectus ac risus ullamcorper suscipit. Suspendisse et erat id odio mollis volutpat non quis ante. Vestibulum posuere commodo augue, ut mattis turpis bibendum nec. Mauris scelerisque, est ac tempus consectetur, risus tortor facilisis ante, non faucibus felis ex at nibh. Vivamus interdum, mauris in tincidunt venenatis, elit purus tempus diam, sit amet euismod ligula massa vel nibh. Donec vulputate consequat tempus. Curabitur id vulputate nibh.",
        "creator": "cayke@instabuy.com.br",
        "id": "5cf979f44360c52896606ca2",
        "last_modified": "2019-06-06T20:39:16.818000+00:00"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /module`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`title` | string | required | Module title.
`description` | string | required | Module description.
`image` | File | optional | Module image.
`answer_id` | string | required | Answer where module should be added.

# Question
This API allows you to CRUD questions.

## Question properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`title` | string | User email.
`is_flow_root` | boolean | If this question is the root of app flow.
`choices` | array | List of possible answers. See [Answer](#answer-properties).

## Retrieve Question Tree
This API return question data and all children in the tree.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_admin_v1/question' \
  -b 'GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8; 
```

> JSON response example:

```json
{
    "data": [
        {
            "title": "Você tem alguma ideia de negócio?",
            "is_flow_root": true,
            "created_at": "2019-06-06T19:26:55.916000+00:00",
            "choices": [
                {
                    "title": "Não",
                    "created_at": "2019-06-06T19:37:01.286000+00:00",
                    "finish_module": {
                        "title": "Modulo Curiosidade",
                        "created_at": "2019-06-06T20:39:16.818000+00:00",
                        "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ultrices varius nisl, vitae volutpat lorem ultricies in. Nunc accumsan porta nulla, id maximus justo. Aliquam erat volutpat. Mauris in nisi scelerisque, accumsan libero in, posuere ante. Nam euismod efficitur feugiat. Maecenas vestibulum, mi quis dapibus semper, nisl lorem varius metus, bibendum convallis dui risus viverra tellus. Nam placerat hendrerit quam vel suscipit. Mauris sagittis lectus ac risus ullamcorper suscipit. Suspendisse et erat id odio mollis volutpat non quis ante. Vestibulum posuere commodo augue, ut mattis turpis bibendum nec. Mauris scelerisque, est ac tempus consectetur, risus tortor facilisis ante, non faucibus felis ex at nibh. Vivamus interdum, mauris in tincidunt venenatis, elit purus tempus diam, sit amet euismod ligula massa vel nibh. Donec vulputate consequat tempus. Curabitur id vulputate nibh.",
                        "creator": "cayke@instabuy.com.br",
                        "id": "5cf979f44360c52896606ca2",
                        "last_modified": "2019-06-06T20:39:16.818000+00:00"
                    },
                    "creator": "cayke@instabuy.com.br",
                    "id": "5cf96b5d1ed86d47bc64c75b",
                    "last_modified": "2019-06-06T20:39:16.832000+00:00"
                },
                {
                    "title": "Sim",
                    "created_at": "2019-06-06T19:33:40.738000+00:00",
                    "creator": "cayke@instabuy.com.br",
                    "id": "5cf96a946110d035057e36f7",
                    "last_modified": "2019-06-06T20:44:07.633000+00:00",
                    "next_question": {
                        "title": "Já possui negócio?",
                        "is_flow_root": false,
                        "created_at": "2019-06-06T20:44:07.629000+00:00",
                        "choices": [],
                        "creator": "cayke@instabuy.com.br",
                        "id": "5cf97b174360c52896606ca3",
                        "last_modified": "2019-06-06T20:44:07.629000+00:00"
                    }
                }
            ],
            "creator": "cayke@instabuy.com.br",
            "id": "5cf968ff54d723678ade3adf",
            "last_modified": "2019-06-06T19:37:01.292000+00:00"
        }
    ],
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /question`

## Create Question
```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/question \
  -H 'cookie: GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8' \
  -F 'title=Já possui negócio?' \
  -F is_flow_root=false \
  -F father_answer_id=5cf96a946110d035057e36f7
```

> JSON response example:

```json
{
    "data": {
        "title": "Já possui negócio?",
        "is_flow_root": false,
        "created_at": "2019-06-06T20:44:07.629000+00:00",
        "choices": [],
        "creator": "cayke@instabuy.com.br",
        "id": "5cf97b174360c52896606ca3",
        "last_modified": "2019-06-06T20:44:07.629000+00:00"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /question`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`title` | string | required | Question title.
`is_flow_root` | bool | required | If question is the root of the flow.
`father_answer_id` | string | optional | Answer where question should be added.

OBS: `father_answer_id` is required if is_flow_root is `false`.


# States and Cities
This API allows you to get states and cities.

## Retrieve States
This API returns all states.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_client_v1/states'
```

> JSON response example:

```json
{
    "data": [
        {
            "state": "AC",
            "name": "Acre"
        },
        {
            "state": "AL",
            "name": "Alagoas"
        },
        {
            "state": "AM",
            "name": "Amazonas"
        },
        {
            "state": "AP",
            "name": "Amapá"
        },
        {
            "state": "BA",
            "name": "Bahia"
        },
        {
            "state": "CE",
            "name": "Ceará"
        },
        {
            "state": "DF",
            "name": "Distrito Federal"
        },
        {
            "state": "ES",
            "name": "Espírito Santo"
        },
        {
            "state": "GO",
            "name": "Goiás"
        },
        {
            "state": "MA",
            "name": "Maranhão"
        },
        {
            "state": "MG",
            "name": "Minas Gerais"
        },
        {
            "state": "MS",
            "name": "Mato Grosso do Sul"
        },
        {
            "state": "MT",
            "name": "Mato Grosso"
        },
        {
            "state": "PA",
            "name": "Pará"
        },
        {
            "state": "PB",
            "name": "Paraíba"
        },
        {
            "state": "PE",
            "name": "Pernambuco"
        },
        {
            "state": "PI",
            "name": "Piauí"
        },
        {
            "state": "PR",
            "name": "Paraná"
        },
        {
            "state": "RJ",
            "name": "Rio de Janeiro"
        },
        {
            "state": "RN",
            "name": "Rio Grande do Norte"
        },
        {
            "state": "RO",
            "name": "Rondônia"
        },
        {
            "state": "RR",
            "name": "Roraima"
        },
        {
            "state": "RS",
            "name": "Rio Grande do Sul"
        },
        {
            "state": "SC",
            "name": "Santa Catarina"
        },
        {
            "state": "SE",
            "name": "Sergipe"
        },
        {
            "state": "SP",
            "name": "São Paulo"
        },
        {
            "state": "TO",
            "name": "Tocantins"
        }
    ],
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /states`

## Retrieve Cities
This API returns cities from state.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_client_v1/states'
  -F state=AC
```

> JSON response example:

```json
{
    "data": [
        "Acrelândia",
        "Assis Brasil",
        "Brasiléia",
        "Bujari",
        "Capixaba",
        "Cruzeiro do Sul",
        "Epitaciolândia",
        "Feijó",
        "Jordão",
        "Mâncio Lima",
        "Manoel Urbano",
        "Marechal Thaumaturgo",
        "Plácido de Castro",
        "Porto Acre",
        "Porto Walter",
        "Rio Branco",
        "Rodrigues Alves",
        "Santa Rosa do Purus",
        "Sena Madureira",
        "Senador Guiomard",
        "Tarauacá",
        "Xapuri",
        "Campinas"
    ],
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /states`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`state` | string | required | State.


# User
This API allows you to create, edit and get users.

## User properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`email` | string | User email.

## Retrieve User
This API allows you to get the logged user.

```shell
curl -X GET \
  http://api.gamelab.tk/api_admin_v1/user \
  -H 'cookie: GLAdminSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
```

> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /user`

# User Password
This API allows you to change user password with different methods.

## Change with actual password
This API replaces the logged user password if password matches to actual password.

```shell
curl -X PUT \
  http://api.gamelab.tk/api_admin_v1/user \
  -H 'cookie: GLAdminSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708' \
  -F password=123456 \
  -F new_password=qwerty
```

> JSON response example:

```json
{  
   "data":"ok",
   "count":0,
   "http_status":200
}
``` 

### HTTP Request
`PUT /user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`password` | string | required | User actual password.
`new_password` | string | required | User new password.

## Generate Token 
This API generates token and send it to user email.

```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/forgotpassword \
  -F email=cayke10@gmail.com
```

> JSON response example:

```json
{  
   "data":"ok",
   "count":0,
   "http_status":200
}
``` 

### HTTP Request
`POST /forgotpassword`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | required | User email.


## Change Password with Token
Changes User password using Token send to email.

```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/change_password \
  -F token=6a715cce7d9011e981d2f01898ee8fb8888c8a720b134a63b568d3f4487cc947 \
  -F password=qwerty
```

> JSON response example:

```json
{  
   "data":"ok",
   "count":0,
   "http_status":200
}
``` 

### HTTP Request
`POST /change_password`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`password` | string | required | New password.
`token` | string | required | Token send to user email.
