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

## Images
All fields that represent images have only the image Identifier and not the image URL.
<!-- todo -->

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

# Question
This API allows you to list questions by id.

## Question properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`title` | string | User email.
`is_flow_root` | boolean | If this question is the root of app flow.
`choices` | array | List of possible answers. See [Answer](#answer-properties).

## Retrieve Question
This API return question data.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_client_v1/question?id=5cf97b174360c52896606ca3' \
  -b 'GLAdminSessionCookie=5c15f7287aa145eaab464b4d597ab0f8; 
```

> JSON response example:

```json
{
    "data": {
        "title": "Você tem alguma ideia de negócio?",
        "is_flow_root": true,
        "choices": [
            {
                "title": "Não",
                "finish_module": "5cf979f44360c52896606ca2",
                "id": "5cf96b5d1ed86d47bc64c75b"
            },
            {
                "title": "Sim",
                "id": "5cf96a946110d035057e36f7",
                "next_question": "5cf97b174360c52896606ca3"
            }
        ],
        "id": "5cf968ff54d723678ade3adf"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /question`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`id` | string | optional | Question Id.


# Screening
This API allows you to go through idea initial setup.

## Get actual question
This API allows you to get the current question on screening flow.

```shell
curl -X GET \
  http://api.gamelab.tk/api_client_v1/screening \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
```

> JSON response example:

```json
{
    "data": {
        "title": "Você tem alguma ideia de negócio?",
        "id": "5cf968ff54d723678ade3adf",
        "is_flow_root": true,
        "choices": [
            {
                "title": "Não",
                "id": "5cf96b5d1ed86d47bc64c75b",
                "finish_module": "5cf979f44360c52896606ca2"
            },
            {
                "title": "Sim",
                "id": "5cf96a946110d035057e36f7",
                "next_question": "5cf97b174360c52896606ca3"
            }
        ]
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /screening`

## Answer Question
This API allows you to get the answer a question. The response is either a new [Question](#question-properties) or a [Module](#module-properties).

```shell
curl -X POST \
  http://api.gamelab.tk/api_client_v1/screening \
  -H 'cookie: GLClientSessionCookie=06564ea3fefe4b568d5867dd26d4bd82' \
  -d 'question_id=5cf968ff54d723678ade3adf&answer_id=5cf96a946110d035057e36f7'
```

> JSON response example:

```json
{
    "data": {
        "title": "Já possui negócio?",
        "id": "5cf97b174360c52896606ca3",
        "choices": [],
        "is_flow_root": false
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /screening`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`question_id` | string | required | Question Id.
`answer_id` | string | required | Choiced answer Id.

# User
This API allows you to create, edit and get users.

## User properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`email` | string | User email.
`phone` | string | User phone.
`user_type` | string | User type. If `PF` see [UserPF](#userpf-properties), if `PJ` see [UserPJ](#userpj-properties).
`gender` | string | User gender. Either `F` or `M`.
`first_name` | string | User first name.
`last_name` | string | User last name.
`birthday` | datetime | User birthday.
`cpf` | string | User CPF.
`zipcode` | string | Address zipcode.
`state` | string | Address state.
`city` | string | Address city.
`neighborhood` | string | Address neighborhood.
`street` | string | Adress Street name.
`street_number` | string | Address Street/House Number.
`complement` | string | Address complement.

## UserPF properties
All UserPF inherit from User.

## UserPJ properties
All UserPJ inherit from User.

Attribute | Type | Description
-------------- | -------------- | -------------- 
`company_name` | string | User company name.
`company_size` | string | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | User CNPJ.

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
`phone` | string | required | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | required | User first name.
`last_name` | string | required | User last name.
`gender` | string | required | User gender. `F` or `M`.
`birthday` | string | required | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required | User CPF. `XXX.XXX.XXX-XX` format.
`zipcode` | string | required | Address zipcode.
`state` | string | required | Address state. `XX` format.
`city` | string | required | Address city.
`neighborhood` | string | required | Address neighborhood.
`street` | string | required | Adress Street name.
`street_number` | string | required | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | required for PJ | User company name.
`company_size` | string | required for PJ | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | required for PJ | User CNPJ. `XX.XXX.XXX/XXXX-XX` format.

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
`phone` | string | required | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | required | User first name.
`last_name` | string | required | User last name.
`gender` | string | required | User gender. `F` or `M`.
`birthday` | string | required | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required | User CPF. `XXX.XXX.XXX-XX` format.
`zipcode` | string | required | Address zipcode.
`state` | string | required | Address state. `XX` format.
`city` | string | required | Address city.
`neighborhood` | string | required | Address neighborhood.
`street` | string | required | Adress Street name.
`street_number` | string | required | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | required for PJ | User company name.
`company_size` | string | required for PJ | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | required for PJ | User CNPJ. `XX.XXX.XXX/XXXX-XX` format.

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
