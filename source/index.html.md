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

Welcome to GameLab's **REST** Client API.

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
This API allow you to login user.

```shell
curl -X POST \
  http://localhost:8000/api_client_v1/login \
  -F email=cayke10@gmail.com \
  -F password=123456
```

> JSON response example:

```json
{
    "data": {
        "id": "5cddb6dfb4876c265729c75e",
        "email": "cayke10@gmail.com",
        "phone": "(61)99161-3871",
        "first_name": "Cayke",
        "last_name": "Prudente",
        "gender": "M",
        "cpf": "034.874.481-14",
        "user_type": "PF",
        "birthday": "1993-05-22T12:00:00+00:00",
        "created_at": "2019-05-16T19:15:43.128000+00:00",
        "address": {
            "street": "SQS 412 Bloco B",
            "street_number": "105",
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
`email` | string | required | User email.
`password` | string | required | User password.


# Logout
This API allow you to logout user.

```shell
curl -X DELETE \
  http://localhost:8000/api_client_v1/logout \
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
  http://localhost:8000/api_client_v1/user \
  -H 'cookie: GLClientSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
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
`GET /user`

## Create User
This API allows you to create new User.

```shell
curl -X POST \
  http://localhost:8000/api_client_v1/user \
  -H 'content-length: 1661' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
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
`password` | string | required | User password.
`phone` | string | required | User phone.
`first_name` | string | required | User first name.
`last_name` | string | required | User last name.
`gender` | string | required | User gender. `F` or `M`.
`birthday` | string | required | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required | User CPF.
`zipcode` | string | required | Address zipcode.
`state` | string | required | Address state.
`city` | string | required | Address city.
`neighborhood` | string | required | Address neighborhood.
`street` | string | required | Adress Street name.
`street_number` | string | required | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | required for PJ | User company name.
`company_size` | string | required for PJ | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | required for PJ | User CNPJ.

## Edit User
This API allows you to edit User info.

```shell
curl -X PUT \
  http://localhost:8000/api_client_v1/user \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
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
`PUT /user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`phone` | string | required | User phone.
`first_name` | string | required | User first name.
`last_name` | string | required | User last name.
`gender` | string | required | User gender. `F` or `M`.
`birthday` | string | required | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required | User CPF.
`zipcode` | string | required | Address zipcode.
`state` | string | required | Address state.
`city` | string | required | Address city.
`neighborhood` | string | required | Address neighborhood.
`street` | string | required | Adress Street name.
`street_number` | string | required | Address Street/House Number.
`complement` | string | optional | Address complement.
`company_name` | string | required for PJ | User company name.
`company_size` | string | required for PJ | Company port. `ME`, `EPP`, `MP`, `GP`.
`cnpj` | string | required for PJ | User CNPJ.

# User Password
This API allows you to change user password with different methods.

## Change with actual password
This API replaces the logged user password if password matches to actual password.

```shell
curl -X PUT \
  http://localhost:8000/api_client_v1/user \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
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
`landing_domain` | string | required | To what URL the email sent to user should redirect. The token will be append in the end of this URL.


## Change Password with Token
Changes User password using Token send to email.

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
