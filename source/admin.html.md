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

# Admin
This API allows you to create, list and remove admins.

## List Admins
This API allows you to list admins.

```shell
curl -X GET \
  http://api.gamelab.tk/api_admin_v1/admin \
  -H 'Cookie: GLAdminSessionCookie=154dc1cf15ab43529eb48e20acd78286' \
```

> JSON response example:

```json
{
    "data": [
        {
            "created_at": "2019-10-11T19:02:22.803000+00:00",
            "email": "cayke@instabuy.com.br",
            "creator": "unknown",
            "id": "5cf95de490f8d062f0d4de34",
            "last_modified": "2019-10-11T19:02:22.803000+00:00"
        },
        {
            "created_at": "2019-10-11T19:02:22.803000+00:00",
            "email": "petrosbarreto01@gmail.com",
            "creator": "unknown",
            "id": "5cf97ec590f8d062f0d4de35",
            "last_modified": "2019-10-11T19:02:22.803000+00:00"
        },
        {
            "created_at": "2019-09-20T20:27:04.574000+00:00",
            "email": "cayke@teste.com.br",
            "creator": "cayke@instabuy.com.br",
            "id": "5d8536189bc4dfbe840601c3",
            "last_modified": "2019-09-20T20:27:04.283000+00:00"
        }
    ],
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /admin`

## Create Admin
This API allows you to create new admin.

```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/admin \
  -F email=cayke@teste.com.br \
  -F password=123456
```

> JSON response example:

```json
{
    "data": {
        "id": "5d8536189bc4dfbe840601c3",
        "created_at": "2019-09-20T20:27:04.574000+00:00",
        "creator": "cayke@instabuy.com.br",
        "email": "cayke@teste.com.br",
        "last_modified": "2019-09-20T20:27:04.283000+00:00"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`POST /admin`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | required | User email.
`password` | string | required | User password.

## Remove Admin
This API allows you to delete admin.

```shell
curl -X DELETE \
  'http://api.gamelab.tk/api_admin_v1/admin?id=5da0d00db312aef365a2eaa2' \
  -H 'Cookie:GLAdminSessionCookie=154dc1cf15ab43529eb48e20acd78286' \
```

> JSON response example:

```json
{
    "data": {
        "id": "5d8536189bc4dfbe840601c3",
        "created_at": "2019-09-20T20:27:04.574000+00:00",
        "creator": "cayke@instabuy.com.br",
        "email": "cayke@teste.com.br",
        "last_modified": "2019-09-20T20:27:04.283000+00:00"
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`DELETE /admin`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`id` | string | required | Admin Id.


# Client
This API allows you to list clients.

## List Clients
This API allows you to list clients.

```shell
curl -X GET \
  http://api.gamelab.tk/api_admin_v1/client_user \
  -H 'Cookie: GLAdminSessionCookie=154dc1cf15ab43529eb48e20acd78286' \
```

> JSON response example:

```json
{
    "data": [
        {
            "gender": "M",
            "phone": "(87) 99966-6996",
            "cpf": "115.523.414-61",
            "first_name": "Mario",
            "last_modified": "2019-10-10T11:58:57.416000+00:00",
            "creator": "unknown",
            "last_name": "Silva",
            "id": "5d9f1d0176af65283a15f5ab",
            "email": "mario@email.com",
            "address": {
                "zipcode": "56560-000",
                "city": "Inajá",
                "neighborhood": "Centro",
                "state": "PE",
                "street_number": "280",
                "country": "Brasil",
                "street": "Brasilia",
                "complement": ""
            },
            "created_at": "2019-10-10T11:58:56.801000+00:00",
            "birthday": "1994-04-22T12:00:00+00:00"
        },
        {
            "gender": "M",
            "phone": "(61) 99161-3871",
            "cpf": "034.874.481-14",
            "first_name": "Cayke",
            "last_modified": "2019-10-02T14:35:44.215000+00:00",
            "creator": "unknown",
            "last_name": "Prudente ",
            "id": "5d94b5bfafcba37c3e9c9d27",
            "email": "cayke10@gmail.com",
            "address": {
                "zipcode": "70278-020",
                "city": "Brasília",
                "neighborhood": "Asa Sul",
                "state": "DF",
                "street_number": "100",
                "country": "Brasil",
                "street": "SQS 412 Bloco B",
                "complement": ""
            },
            "created_at": "2019-10-02T14:35:43.551000+00:00",
            "birthday": "1993-05-22T12:00:00+00:00"
        },
        {
            "gender": "M",
            "phone": "(87) 9966-6699",
            "cpf": "115.523.414-61",
            "first_name": "Vanildo",
            "last_modified": "2019-09-27T13:48:03.316000+00:00",
            "creator": "unknown",
            "last_name": "Júnior",
            "id": "5d8e13121a74f4ada2b0c2d6",
            "email": "vanildojr3333@gmail.com",
            "address": {
                "zipcode": "55292-220",
                "city": "Garanhuns",
                "neighborhood": "Aloísio Souto Pinto",
                "state": "PE",
                "street_number": "280",
                "country": "Brasil",
                "street": "Rua José Bonifácio",
                "complement": ""
            },
            "created_at": "2019-09-27T13:48:02.657000+00:00",
            "birthday": "1997-06-24T12:00:00+00:00"
        },
        {
            "gender": "M",
            "phone": "(87) 99966-6699",
            "cpf": "119.168.824-04",
            "first_name": "Gabriel",
            "last_modified": "2019-09-24T19:31:23.872000+00:00",
            "creator": "unknown",
            "last_name": "Castro",
            "id": "5d8a6f0bf9e82a9ea09a1069",
            "email": "gabriel@alivesi.com.br",
            "address": {
                "zipcode": "55292-220",
                "city": "Garanhuns",
                "neighborhood": "Aloísio Souto Pinto",
                "state": "PE",
                "street_number": "280",
                "country": "Brasil",
                "street": "Rua José Bonifácio",
                "complement": ""
            },
            "created_at": "2019-09-24T19:31:23.246000+00:00",
            "birthday": "1997-06-24T12:00:00+00:00"
        },
        {
            "gender": "M",
            "phone": "(87) 99966-6699",
            "cpf": "115.523.414-61",
            "first_name": "Vanildo",
            "last_modified": "2019-09-24T19:28:11.437000+00:00",
            "creator": "unknown",
            "last_name": "Junior",
            "id": "5d8a6e4bf9e82a9ea09a1068",
            "email": "vanildo@alivesi.com.br",
            "address": {
                "zipcode": "55292-220",
                "city": "Garanhuns",
                "neighborhood": "Aloísio Souto Pinto",
                "state": "PE",
                "street_number": "280",
                "country": "Brasil",
                "street": "Rua José Bonifácio",
                "complement": ""
            },
            "created_at": "2019-09-24T19:28:10.793000+00:00",
            "birthday": "1997-06-24T12:00:00+00:00"
        }
    ],
    "count": 5,
    "http_status": 200
}
```

### HTTP Request
`GET /client_user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | optional | User email.
`cpf` | string | optional | User cpf.
`items_per_page` | string | int | Items per page.
`page` | string | int | Query page - 1 based index.

## Delete Client
This API allows you to delete clients.

```shell
curl -X DELETE \
  http://api.gamelab.tk/api_admin_v1/client_user \
  -H 'Cookie: GLAdminSessionCookie=154dc1cf15ab43529eb48e20acd78286' \
  -F email=cayke10@gmail.com
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
`DELETE /client_user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | optional | User email.
`cpf` | string | optional | User cpf.

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


# User
This API allows you to get and edit logged user.

## User properties
Attribute | Type | Description
-------------- | -------------- | -------------- 
`id` | string |	Unique identifier for the resource.
`email` | string | User email.
`phone` | string | User phone. `(XX)XXXXX-XXXX` format.
`first_name` | string | User first name.
`last_name` | string | User last name.
`about` | string | User about.

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


# Report
This API allows you to generate system reports.

## Retrieve Report
This API allows you to get the logged user.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_admin_v1/report?sex=1&age=1&state=1&initial_state=1&actual_phase=1' \
  -H 'cookie: GLAdminSessionCookie=28a9f2fe1c5e40f8bfbba687f245f708'
```

> JSON response example:

```json
{
    "data": {
        "sex": {
            "male_count": 3,
            "female_count": 0,
            "total_count": 3
        },
        "age": {
            "total_count": 3,
            "17-": 0,
            "18-24": 3,
            "25-34": 0,
            "35-44": 0,
            "45-54": 0,
            "55-64": 0,
            "65+": 0
        },
        "state": {
            "total_count": 3,
            "PE": 3
        },
        "initial_state": {
            "total_count": 2,
            "ideation": 2
        },
        "actual_phase": {
            "total_count": 2,
            "0": 2
        }
    },
    "count": 0,
    "http_status": 200
}
```

### HTTP Request
`GET /report`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`sex` | string | optional | Pass it if you want sex report.
`age` | string | optional | Pass it if you want age report.
`state` | string | optional | Pass it if you want state report.
`cities` | string | optional | Pass it if you want cities report. Must pass state identifier on `cities` parameter. Ex: `cities=DF`.
`initial_state` | string | optional | Pass it if you want idea/company initial state report.
`actual_phase` | string | optional | Pass it if you want users actual game phase report.
`section` | string | optional | Pass it if you want section report.
`pains` | string | optional | Pass it if you want pains report.
`users_count` | string | optional | Pass it if you want users count from the last 12 months.