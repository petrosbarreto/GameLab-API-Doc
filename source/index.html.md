---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  # - shell
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

> JSON response example:

```json
{  
   "data":{  
      "first_name":"Cayke",
      "id":"56d1d826072d4127fb6fb9f9",
      "addresses":[],
      "last_name":"Prudente",
      "cpf":"034.123.765-14",
      "birthday":"1990-04-21T12:00:00+00:00",
      "user_type":"PF",
      "phone":"(61)99999-3871",
      "email":"cayke@gmail.com",
      "gender":"M",
      "cards":[]
   },
   "status":"success",
   "count":0,
   "http_status":200
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

> JSON response example:

```json
{  
   "data":"ok",
   "status":"success",
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

## UserPF properties
All UserPF inherit from User.

Attribute | Type | Description
-------------- | -------------- | -------------- 
`gender` | string | User gender. Either `F` or `M`.
`first_name` | string | User first name.
`last_name` | string | User last name.
`birthday` | datetime | User birthday.
`cpf` | string | User CPF.

## UserPJ properties
All UserPJ inherit from User.

Attribute | Type | Description
-------------- | -------------- | -------------- 
`company_name` | string | User company name.
`fantasy_name` | string | User fantasy name.
`cnpj` | string | User CNPJ.

## Retrieve User
This API allows you to get the logged user.

> JSON response example:

```json
{  
   "data":{  
      "first_name":"Cayke",
      "id":"56d1d826072d4127fb6fb9f9",
      "last_name":"Prudente",
      "cpf":"034.123.765-14",
      "birthday":"1990-04-21T12:00:00+00:00",
      "user_type":"PF",
      "phone":"(61)99999-3871",
      "email":"cayke@gmail.com",
      "gender":"M"
   },
   "count":0,
   "http_status":200
}
```

### HTTP Request
`GET /user`

## Create User
This API allows you to create new User.

```json
{  
   "data":{  
      "first_name":"Cayke",
      "id":"56d1d826072d4127fb6fb9f9",
      "last_name":"Prudente",
      "cpf":"034.123.765-14",
      "birthday":"1990-04-21T12:00:00+00:00",
      "user_type":"PF",
      "phone":"(61)99999-3871",
      "email":"cayke@gmail.com",
      "gender":"M",
   },
   "count":0,
   "http_status":200
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
`first_name` | string | required for PF | User first name.
`last_name` | string | required for PF | User last name.
`gender` | string | required for PF | User gender. `F` or `M`.
`birthday` | string | required for PF | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required for PF | User CPF.
`company_name` | string | required for PJ | User company name.
`fantasy_name` | string | required for PJ | User fantasy name.
`cnpj` | string | required for PJ | User CNPJ.

## Edi User
This API allows you to edit User info.

```json
{  
   "data":{  
      "first_name":"Cayke",
      "id":"56d1d826072d4127fb6fb9f9",
      "last_name":"Prudente",
      "cpf":"034.123.765-14",
      "birthday":"1990-04-21T12:00:00+00:00",
      "user_type":"PF",
      "phone":"(61)99999-3871",
      "email":"cayke@gmail.com",
      "gender":"M"
   },
   "count":0,
   "http_status":200
}
```

### HTTP Request
`PUT /user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`phone` | string | required | User phone.
`first_name` | string | required for PF | User first name.
`last_name` | string | required for PF | User last name.
`gender` | string | required for PF | User gender. `F` or `M`.
`birthday` | string | required for PF | User birthday. `DD/MM/YYYY` format.
`cpf` | string | required for PF | User CPF.
`company_name` | string | required for PJ | User company name.
`fantasy_name` | string | required for PJ | User fantasy name.
`cnpj` | string | required for PJ | User CNPJ.

# User Password
This API allows you to change user password with different methods.

## Change with actual password
This API replaces the logged user password if password matches to actual password.

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
