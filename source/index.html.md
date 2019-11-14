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

Welcome to GameLab's REST **Client** API.

To check other docs, click above:

- [Admin Doc](/admin.html).

## URLs

For development purpose you should use [http://api.gamelab.com.br/api_client_v1](http://api.gamelab.com.br/api_client_v1).


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

# Images
All fields that represent images have only the image Identifier and not the image URL.

To acess the image, append the image key to the url: [https://gamelab-files.s3.amazonaws.com/`image_key`](https://gamelab-files.s3.amazonaws.com/).

## Upload image
To upload an image, send the image as a binary to the endpoint above.

```shell
curl -X POST \
  http://localhost:8000/api_client_v1/image \
  -H 'Content-Length: 34191' \
  -H 'Cookie: GLClientSessionCookie=5adfd91663f740eb81c6d4924bd507e1' \
  -F 'file=@/Users/cayke/Desktop/Captura de Tela 2019-11-12 às 16.27.58.png'
```

> JSON response example:

```json
{
    "data": "58bada0915e440548852a8df5b0821ad.png",
    "count": 0,
    "http_status": 200
}
```
### HTTP Request
`POST /image`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`file` | binary | required | `PNG` or `JPG` image. Max size is `2 MB`.


# Authentication

The Authentication in GameLab's API is made using **COOKIES**. Therefore, to log in and access protected endpoints you must have cookies enabled on your browser/app.


# Login
This API allow you to login user. You must pass `email` and `password` or `fb_user_id`.

```shell
curl -X POST \
  http://api.gamelab.tk/api_client_v1/login \
  -F email=cayke10@gmail.com \
  -F password=123456
```

> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com",
        "phone": "(61)99161-3771",
        "first_name": "Cayke",
        "last_name": "Prudente",
        "gender": "M",
        "cpf": "034.834.481-24",
        "user_type": "PF",
        "birthday": "1993-05-22T12:00:00+00:00",
        "created_at": "2019-05-16T19:15:43.128000+00:00",
        "address": {
            "street": "SQS 412 Bloco X",
            "street_number": "100",
            "zipcode": "70278020",
            "neighborhood": "Asa sul",
            "complement": "",
            "state": "DF",
            "city": "Brasilia"
        }
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
`fb_user_id` | string | optional | User Facebook Id.


# Logout
This API allow you to logout user.

```shell
curl -X DELETE \
  http://api.gamelab.tk/api_client_v1/logout \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
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


# Progress
This API allows you to save user progress through the game.

## Progress properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`coins` | int | User coins.
`phase` | string | User last game phase.
`initial_state` | string | Company inital state after first quiz. `curiosity`, `ideation`,  `operation` or `traction`.
`section` | string | Company section/market.
`pains` | array | List with strings. What pains company solve.
`target_audience` | string |  Company target audience.
`target_justification` | string | Company target audience justification.
`persona` | array | List with strings. Company persona infos.
`channels` | array |  List with strings. Company acquisition channels.
`mkt_monthly_budget` | float | Company marketing budget.
`sale_monthly_budget` | float | Company sale budget.
`new_clients_per_month` | int | New clients per month.
`recipe_model` | string | How company makes money.
`user_idea` | string | User idea.
`validation_missions` | array | Array with validation missions.
`experiments` | array | List with [Experiment Boards](#experiment-board-properties).

## Experiment Board properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`client` | string |	Who is the test for.
`hypothesis` | string | What risk is this test testing?
`test` | string | What is the smallest experiment I can do to test this hypothesis?
`success_criterion` | string | What is the criterion for you to consider the experiment a success?
`result` | string | Result x criterion (passed or failed)
`learning` | string | What did you learn from this experiment?


## Retrieve Progress
This API returns current progress.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_client_v1/progress'
```

> JSON response example:

```json
{
    "data": {
        "id": "5d7ac525a86bf6aaef92bc03",
        "coins": 0,
        "phase": "0",
        "user": "5cddb6dfb4876c265729c75e"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /progress`

## Edit Progress
This API allows you to edit user progress.

```shell
curl -X PUT \
  http://api.gamelab.tk/api_client_v1/progress \
  -H 'Cookie:GLClientSessionCookie=4d1b50721ed4447fbcab6556da74cdd6' \
  -F coins=50 \
  -F initial_state=ideation
```
> JSON response example:

```json
{
    "data": {
        "user": "5cddb6dfb4876c265729c75e",
        "id": "5d7ac525a86bf6aaef92bc03",
        "coins": 50,
        "initial_state": "ideation",
        "phase": "0"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`PUT /progress`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`coins` | int | optional | User coins.
`phase` | string | optional | User last game phase.
`initial_state` | string | optional | Company inital state after first quiz. `curiosity`, `ideation`,  `operation` or `traction`.
`section` | string | optional | Company section/market.
`pains` | array | optional | List with strings. What pains company solve.
`target_audience` | string |  optional | Company target audience.
`target_justification` | string | optional | Company target audience justification.
`persona` | array | optional | List with strings. Company persona infos.
`channels` | array |  optional | List with strings. Company acquisition channels.
`mkt_monthly_budge` | float | optional | Company marketing budget.
`sale_monthly_budge` | float | optional | Company sale budget.
`new_clients_per_month` | int | optional | New clients per month.
`recipe_model` | string | optional | How company makes money.
`user_idea` | string | optional | User idea.
`validation_missions` | array | optional | Array with validation missions.
`experiment` | object | optional | Pass the `phase` field (from 0 to 4) plus the [Experiment Board Object](#experiment-board-properties).

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
`phone` | string | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | User first name.
`last_name` | string | User last name.
`gender` | string | User gender. `F` or `M`.
`birthday` | string | User birthday. `DD/MM/YYYY` format.
`cpf` | string | User CPF. `XXX.XXX.XXX-XX` format.
`zipcode` | string | Address zipcode.
`state` | string | Address state. `XX` format.
`city` | string | Address city.
`neighborhood` | string | Address neighborhood.
`street` | string | Adress Street name.
`street_number` | string | Address Street/House Number.
`complement` | string | Address complement.
`company_name` | string | User company name.
`fantasy_name` | string | User fantast name.
`company_size` | string | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | User CNPJ. `XX.XXX.XXX/XXXX-XX` format.
`latitude` | float | User location latitude.
`longitude` | float | User location longitude.

## Retrieve User
This API allows you to get the logged user.

```shell
curl -X GET \
  http://api.gamelab.tk/api_client_v1/user \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
```

> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com",
        "phone": "(61)99161-3771",
        "first_name": "Cayke",
        "last_name": "Prudente",
        "gender": "M",
        "cpf": "034.834.481-24",
        "user_type": "PF",
        "birthday": "1993-05-22T12:00:00+00:00",
        "created_at": "2019-05-16T19:15:43.128000+00:00",
        "address": {
            "street": "SQS 412 Bloco X",
            "street_number": "100",
            "zipcode": "70278020",
            "neighborhood": "Asa sul",
            "complement": "",
            "state": "DF",
            "city": "Brasilia"
        }
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /user`

## Create User
This API allows you to create new User.

```shell
curl -X POST \
  http://api.gamelab.tk/api_client_v1/user \
  -F email=cayke10@gmail.com \
  -F password=123456 \
  -F first_name=Cayke \
  -F last_name=Prudente \
  -F gender=M \
  -F birthday=22/05/1993 \
  -F cpf=034.874.481-14 \
  -F 'phone=(61)99161-3871' \
  -F zipcode=70278020 \
  -F state=DF \
  -F city=Brasilia \
  -F 'neighborhood=Asa sul' \
  -F 'street=SQS 412 Bloco B' \
  -F street_number=105
```

> JSON response example:

```json
{
    "data": {
        "email": "cayke10@gmail.com",
        "address": {
            "complement": "",
            "street": "SQS 412 Bloco B",
            "street_number": "105",
            "city": "Brasilia",
            "neighborhood": "Asa sul",
            "state": "DF",
            "zipcode": "70278020"
        },
        "id": "5cddb6dfb4876c265729c75e",
        "last_name": "Prudente",
        "phone": "(61)99161-3871",
        "cpf": "034.874.481-14",
        "birthday": "1993-05-22T12:00:00+00:00",
        "created_at": "2019-05-16T19:15:43.128000+00:00",
        "gender": "M",
        "first_name": "Cayke",
        "user_type": "PF"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | required | User email.
`password` | string | optional | User password.
`fb_user_id` | string | optional | User Facebook Id.
`phone` | string | optional | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | optional | User first name.
`last_name` | string | optional | User last name.
`gender` | string | optional | User gender. `F` or `M`.
`birthday` | string | optional | User birthday. `DD/MM/YYYY` format.
`cpf` | string | optional | User CPF. `XXX.XXX.XXX-XX` format.
`zipcode` | string | optional | Address zipcode.
`state` | string | optional | Address state. `XX` format.
`city` | string | optional | Address city.
`neighborhood` | string | optional | Address neighborhood.
`street` | string | optional | Adress Street name.
`street_number` | string | optional | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | optional | User company name.
`fantasy_name` | string | optional | User fantast name.
`company_size` | string | optional | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | optional | User CNPJ. `XX.XXX.XXX/XXXX-XX` format.
`latitude` | float | optional | User location latitude.
`longitude` | float | optional | User location longitude.

## Edit User
This API allows you to edit User info.

```shell
curl -X PUT \
  http://api.gamelab.tk/api_client_v1/user \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708' \
  -F first_name=Cayke \
  -F last_name=Prudente \
  -F gender=M \
  -F birthday=22/05/1993 \
  -F cpf=034.834.481-24 \
  -F 'phone=(61)99161-3771' \
  -F zipcode=70278020 \
  -F state=DF \
  -F city=Brasilia \
  -F 'neighborhood=Asa sul' \
  -F 'street=SQS 412 Bloco X' \
  -F street_number=100
```
> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com",
        "phone": "(61)99161-3771",
        "first_name": "Cayke",
        "last_name": "Prudente",
        "gender": "M",
        "cpf": "034.834.481-24",
        "user_type": "PF",
        "birthday": "1993-05-22T12:00:00+00:00",
        "created_at": "2019-05-16T19:15:43.128000+00:00",
        "address": {
            "street": "SQS 412 Bloco X",
            "street_number": "100",
            "zipcode": "70278020",
            "neighborhood": "Asa sul",
            "complement": "",
            "state": "DF",
            "city": "Brasilia"
        }
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`PUT /user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`phone` | string | optional | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | optional | User first name.
`last_name` | string | optional | User last name.
`gender` | string | optional | User gender. `F` or `M`.
`birthday` | string | optional | User birthday. `DD/MM/YYYY` format.
`cpf` | string | optional | User CPF. `XXX.XXX.XXX-XX` format.
`zipcode` | string | optional | Address zipcode.
`state` | string | optional | Address state. `XX` format.
`city` | string | optional | Address city.
`neighborhood` | string | optional | Address neighborhood.
`street` | string | optional | Adress Street name.
`street_number` | string | optional | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | optional | User company name.
`fantasy_name` | string | optional | User fantast name.
`company_size` | string | optional | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | optional | User CNPJ. `XX.XXX.XXX/XXXX-XX` format.
`latitude` | float | optional | User location latitude.
`longitude` | float | optional | User location longitude.

# User Password
This API allows you to change user password with different methods.

## Change with actual password
This API replaces the logged user password if password matches to actual password.

```shell
curl -X PUT \
  http://api.gamelab.tk/api_client_v1/user \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708' \
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
  http://api.gamelab.tk/api_client_v1/forgotpassword \
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
  http://api.gamelab.tk/api_client_v1/change_password \
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


# Zipcode
This API allows you to complete address info using zipcode.

## Retrieve Address by zipcode
This API returns an [Address](#user-properties).

```shell
curl -X GET \
  'http://api.gamelab.tk/api_client_v1/zipcode?zipcode=70278020'
```

> JSON response example:

```json
{
  "data": {
    "zipcode": "70278-020",
    "street": "SQS 412 Bloco B",
    "neighborhood": "Asa Sul",
    "city": "Brasília",
    "state": "DF"
  },
  "count": 0,
  "http_status": 200
}
```

### HTTP Request
`GET /zipcode`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`zipcode` | string | required | Zipcode.