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
`user_type` | string | required | User type. Opts: `super`, `normal`.

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


## List Clients based on progress
This API allows you to list clients based on progress key/value.

```shell
curl -X GET \
  'http://api.gamelab.tk/api_admin_v1/client_user?key=initial_state&value=curiosity&start_date=2019-12-10&end_date=2019-12-31' \
  -H 'Cookie: GLAdminSessionCookie=c0cb02a987ef427c8d1a6ea7620362b8' \
```

> JSON response example:

```json
{
    "data": [
        [
            {
                "email": "jose.nvillalba12@gmail.com",
                "creator": "unknown",
                "password": "$2b$12$usLQKsYoujrbD/lcbhp0KuM6gzO0lWFLnVLdCsI1y2agEZn/gPzui",
                "created_at": "2019-12-10T19:25:14.438000",
                "blocked": false,
                "last_modified": "2019-12-10T20:29:42.636000",
                "progress": "5deff1fdcf4041831120af50",
                "address": {
                    "zipcode": "69912-000",
                    "country": "Brasil",
                    "street": "Rua 26 de Junho",
                    "_cls": "GLAddress",
                    "city": "Rio Branco",
                    "street_number": "300",
                    "state": "AC",
                    "complement": "Casa",
                    "neighborhood": "Sobral"
                },
                "birthday": "1989-05-14T12:00:00",
                "cpf": "231.603.422-55",
                "first_name": "Joao",
                "gender": "M",
                "last_name": "Silva",
                "phone": "(68) 99991-2200",
                "report_s3_id": "7d3c9eb6a451499582c7755bfa3e2b23.pdf",
                "id": "5deff11a579db576a320af51"
            }
        ],
        [
            {
                "email": "deniseoliveira.ac@gmail.com",
                "creator": "unknown",
                "password": "$2b$12$smscDO3JAh3IBr7mKkJtjuo6CMCLUz9e60QrNzevtxLtvgOv7AIsy",
                "created_at": "2019-12-11T04:02:16.711000",
                "blocked": false,
                "last_modified": "2019-12-11T04:04:33.515000",
                "progress": "5df06ad1cf4041831120af75",
                "id": "5df06a48cf4041831120af72"
            }
        ],
        [
            {
                "last_modified": "2019-12-12T01:29:48.333000",
                "created_at": "2019-12-12T01:22:02.337000",
                "email": "ronairacosta@hotmail.com",
                "password": "$2b$12$IVMG9OFm8xmAKfAb1eRD1.gAMPCxmiGyeAqynaKkHnTzetC9c.2Oe",
                "creator": "unknown",
                "blocked": false,
                "progress": "5df1980c6b7b3d5bac7b31c9",
                "id": "5df1963a89e9f79b927b31d3"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$nfloQJL0.znl28O/cAY.A.OJU8H1Rdc17WFcpX9xHajyDTtWnkIwe",
                "email": "j.erickleandro@gmail.com",
                "last_modified": "2019-12-12T14:50:13.902000",
                "blocked": false,
                "created_at": "2019-12-12T14:48:56.849000",
                "progress": "5df253a543b894203c832c29",
                "id": "5df2535943b894203c832c26"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$nvuRb0oYo0rRqLuuudiDX.XP2tBbRPMqfnXHJGHNwhoFNko6Nk2ge",
                "email": "rodsingrid@gmail.com",
                "last_modified": "2019-12-12T15:09:59.900000",
                "blocked": false,
                "created_at": "2019-12-12T15:07:46.382000",
                "progress": "5df2584743b894203c832c34",
                "id": "5df257c243b894203c832c33"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$VHaimkYkPe4Jeq7mgyjaj.KmxHQSs1d4rP0emU4P0wfJYW694Wu9O",
                "email": "darlantc@gmail.com",
                "last_modified": "2019-12-12T15:14:13.944000",
                "blocked": false,
                "created_at": "2019-12-12T15:12:26.124000",
                "progress": "5df2594543b894203c832c37",
                "id": "5df258da2a9c132e42832c22"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$RD9kWukny66IMOja6weQRebhC5MCByaZipG0SEXkpiaGf7G5cLtC2",
                "email": "raulpedrouag@gmail.com",
                "last_modified": "2019-12-12T15:33:14.923000",
                "blocked": false,
                "created_at": "2019-12-12T15:32:14.336000",
                "progress": "5df25dba2a9c132e42832c23",
                "id": "5df25d7e43b894203c832c39"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$s1DhAD.041zZ6Jc9UOPUM.2CsQG7besqTN8eo3yZTD1RDb2z.marC",
                "email": "gcsgivanildo@gmail.com",
                "last_modified": "2019-12-12T16:01:31.572000",
                "blocked": false,
                "created_at": "2019-12-12T15:56:41.337000",
                "progress": "5df2645b43b894203c832c44",
                "id": "5df26339347642bbf2832c22"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$MgAqMWy5mJzj1GcJWFCPdOp8cRePidSL.iOIpTn00VrwtxdejZAx2",
                "email": "milenasantos11508@gmail.com",
                "last_modified": "2019-12-12T16:16:51.533000",
                "blocked": false,
                "created_at": "2019-12-12T16:14:36.655000",
                "progress": "5df267f3347642bbf2832c23",
                "id": "5df2676c43b894203c832c46"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$b44dzQ0ZuvLo6UKpERb/1e7Auro3fBzP4aQ0t7MzQmXw0v9ncRmHu",
                "email": "abcteste@mailinator.com",
                "last_modified": "2019-12-12T17:32:16.009000",
                "blocked": false,
                "created_at": "2019-12-12T17:30:18.343000",
                "progress": "5df279a043b894203c832c4d",
                "id": "5df2792a43b894203c832c4b"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$/ITmEmVByZ3v/SiyBbLwm.hz7tRHBw.X/jUnxr5XAMWp8995VvWaO",
                "email": "sandeison_san@hotmail.com",
                "last_modified": "2019-12-12T17:37:34.418000",
                "blocked": false,
                "created_at": "2019-12-12T17:36:23.819000",
                "progress": "5df27ade43b894203c832c50",
                "id": "5df27a9843b894203c832c4f"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$0DAMkKFtF1wTT6RfQWqhceHSd1HJysagE5/GEV19ySmfDnFwvcVHu",
                "email": "garanhuns@mailinator.com",
                "last_modified": "2019-12-12T17:44:28.892000",
                "blocked": false,
                "created_at": "2019-12-12T17:38:01.383000",
                "progress": "5df27c7c43b894203c832c53",
                "id": "5df27af9ab1b5fb543832c2d"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$rRORamMz60ejPE1cI6n3Zeb3WSXtw8yXrSDl4i4A0AP74uSfgjMQW",
                "email": "aleyne.lins@gmail.com",
                "last_modified": "2019-12-12T19:09:19.354000",
                "blocked": false,
                "created_at": "2019-12-12T19:08:22.639000",
                "progress": "5df2905f43b894203c832c5e",
                "id": "5df2902643b894203c832c5c"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$X6Sh863i/XIyUqy6rfiOwefYb4sY2P5SryoemYAx6J.ze/q7usmlu",
                "blocked": false,
                "last_modified": "2019-12-12T21:57:05.474000",
                "email": "leonardostrn0@gmail.com",
                "created_at": "2019-12-12T21:55:41.108000",
                "progress": "5df2b7b10883c22741a92758",
                "id": "5df2b75d7c687a2935a92758"
            }
        ],
        [
            {
                "email": "muciojaziel@gmail.com",
                "last_modified": "2019-12-13T12:47:42.416000",
                "blocked": false,
                "creator": "unknown",
                "password": "$2b$12$fAoy2bVU.gnU/gZmBQArDuEbomGTO7ybRF2VEChzAmVhFHLVHP3gG",
                "created_at": "2019-12-13T12:44:39.038000",
                "progress": "5df3886e94cb5844e57d4b96",
                "id": "5df387b794cb5844e57d4b94"
            }
        ],
        [
            {
                "email": "teste@cadastro.com",
                "last_modified": "2019-12-13T13:31:59.936000",
                "blocked": true,
                "creator": "unknown",
                "password": "$2b$12$qOxYF0Zeb81IAnIIQblzvuC9eWQ/A1WcA8GINt.9N46jWdFhl4sl2",
                "created_at": "2019-12-13T13:24:59.400000",
                "progress": "5df392cf94cb5844e57d4b9b",
                "id": "5df3912b94cb5844e57d4b98"
            }
        ],
        [
            {
                "email": "teste2@cadastro.com",
                "last_modified": "2019-12-13T13:41:06.439000",
                "blocked": true,
                "creator": "unknown",
                "password": "$2b$12$.q53nPQ.FsIFFtVg9kgdVOAeyCzxPO7VDS/Jc05LnQGQ3eDesJaK2",
                "created_at": "2019-12-13T13:34:54.043000",
                "progress": "5df394f294cb5844e57d4b9e",
                "id": "5df3937e94cb5844e57d4b9c"
            }
        ],
        [
            {
                "email": "testt@cadastro.com",
                "last_modified": "2019-12-13T14:07:14.178000",
                "blocked": true,
                "creator": "unknown",
                "password": "$2b$12$pqK7tfk8eirWD1E4Y23Msu7oPHeUsUhyA5Lcj9CZKB5/WaGF2U9Zi",
                "created_at": "2019-12-13T14:05:37.206000",
                "progress": "5df39b1294cb5844e57d4ba8",
                "id": "5df39ab194cb5844e57d4ba7"
            }
        ],
        [
            {
                "email": "teste2@teste2.com",
                "last_modified": "2019-12-13T16:14:47.110000",
                "blocked": true,
                "creator": "unknown",
                "password": "$2b$12$D9hElT.NjQy7wRsFuzVoGOefAtzdLUFPUvyEGysOEn838QDCBYH06",
                "created_at": "2019-12-13T16:14:41.571000",
                "progress": "5df3b8f794cb5844e57d4bb5",
                "id": "5df3b8f1b0719791177d4b91"
            }
        ],
        [
            {
                "creator": "unknown",
                "password": "$2b$12$f.lfq3AdCFBN26zlk.FhouP9eXNpAVFkjx2PAlP.LOx8YEXRQFfEi",
                "email": "luciana912@gmail.com",
                "last_modified": "2019-12-13T18:05:39.539000",
                "blocked": false,
                "created_at": "2019-12-12T17:27:17.210000",
                "rp_expires_at": "2019-12-12T21:31:57.340000",
                "rp_token": "4be286d81d0511ea99860269f4ad9f245aef22cafa504b8eab8887ed462749ab",
                "progress": "5df3d2f394cb5844e57d4bbc",
                "id": "5df2787543b894203c832c48"
            }
        ],
        [
            {
                "email": "joelontano@icloud.com",
                "last_modified": "2019-12-13T19:30:57.558000",
                "blocked": false,
                "creator": "unknown",
                "password": "$2b$12$Xj8iCqCZ5qlWkehGsLbo9O4xMDfDlxLbi99aovun8qpiNbsz89Upu",
                "created_at": "2019-12-13T14:57:55.828000",
                "progress": "5df3e6f194cb5844e57d4bbf",
                "id": "5df3a6f494cb5844e57d4bac"
            }
        ],
        [
            {
                "email": "pepe@gmail.com",
                "last_modified": "2019-12-14T14:07:45.511000",
                "creator": "unknown",
                "created_at": "2019-12-14T14:07:23.509000",
                "blocked": true,
                "password": "$2b$12$IP/zEWwzBi6az1Gqiu8pMuxLKP2Ee5olOVdE4vhySFWP6Z1m0sJfm",
                "progress": "5df4ecb13ba61f995c13d9b6",
                "id": "5df4ec9b68967f59f013d9b6"
            }
        ],
        [
            {
                "email": "teste123@test.com",
                "last_modified": "2019-12-14T17:30:40.744000",
                "creator": "unknown",
                "created_at": "2019-12-14T17:30:29.553000",
                "blocked": true,
                "password": "$2b$12$89H3qaembbyFEIF.Y64cEuAsVOPCB2W9QWIPOGXpo96MimljUc8ai",
                "progress": "5df51c4068967f59f013d9b8",
                "id": "5df51c3568967f59f013d9b7"
            }
        ],
        [
            {
                "email": "lucas_freitas1999@hotmail.com",
                "last_modified": "2019-12-14T18:29:32.142000",
                "fb_user_id": "2686713128053318",
                "creator": "unknown",
                "created_at": "2019-12-14T18:29:22.425000",
                "blocked": true,
                "progress": "5df52a0c3ba61f995c13d9b7",
                "id": "5df52a0268967f59f013d9c0"
            }
        ],
        [
            {
                "blocked": true,
                "created_at": "2019-12-07T15:32:40.450000",
                "fb_user_id": "2510217879225750",
                "last_modified": "2019-12-14T20:27:46.174000",
                "email": "claudio.roberto196646@gmail.com",
                "creator": "unknown",
                "progress": "5df545c268967f59f013d9cc",
                "id": "5debc618a0e9ad57195036fc"
            }
        ],
        [
            {
                "last_modified": "2019-12-16T19:02:58.852000",
                "fb_user_id": "1468787993288888",
                "email": "paulacdss@hotmail.com",
                "blocked": true,
                "creator": "unknown",
                "created_at": "2019-12-16T19:02:58.183000",
                "progress": "5df7d4e294870f59c736c7b0",
                "id": "5df7d4e294870f59c736c7af"
            }
        ],
        [
            {
                "password": "$2b$12$uzChsvglk1PLtHWRfzSQA.qJ0NHkG90SjlgBnRonR1U/Y4i4cbbfa",
                "last_modified": "2019-12-17T00:01:06.913000",
                "email": "lucas.freitas9935@gmail.com",
                "blocked": false,
                "creator": "unknown",
                "created_at": "2019-12-16T23:52:44.417000",
                "progress": "5df818e0e454a3d12336c7ae",
                "id": "5df818cc94870f59c736c7b7"
            }
        ],
        [
            {
                "password": "$2b$12$RN1tXRXi5vGtDqIqiNt5SOBmy2KHIR5g0uRr/ILt1c90k9eJiYV9K",
                "last_modified": "2019-12-17T03:01:27.067000",
                "email": "mirla12silva@hotmail.com",
                "blocked": true,
                "creator": "unknown",
                "created_at": "2019-12-17T03:01:06.900000",
                "progress": "5df84507eabcd1451836c7ae",
                "id": "5df844f394870f59c736c7ba"
            }
        ]
    ],
    "count": 28,
    "http_status": 200
}
```

### HTTP Request
`GET /client_user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`key` | string | required | Progress key.
`value` | string| required | Progress value.
`start_date` | string | optional | Works with `users_by_section`. `YYYY-MM-DD` format.
`end_date` | string | optional | Works with `users_by_section`. `YYYY-MM-DD` format.

## Create Client
This API allows you to create clients.

```shell
curl -X POST \
  http://api.gamelab.tk/api_admin_v1/client_user \
  -H 'Cookie: GLAdminSessionCookie=154dc1cf15ab43529eb48e20acd78286' \
  -F email=cayke10@gmail.com
```

> JSON response example:

```json
{
    "data": {  
		"last_modified": "2019-10-02T14:35:44.215000+00:00",
		"creator": "unknown",
		"id": "5d94b5bfafcba37c3e9c9d27",
		"email": "cayke10@gmail.com",
		"created_at": "2019-10-02T14:35:43.551000+00:00",
	},
    "count": 1,
    "http_status": 200
}
```

### HTTP Request
`POST /client_user`

#### Available parameters
Parameter | Type | Constraint | Description
-------------- | --------------  | -------------- | -------------- 
`email` | string | required | User email.


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
  http://api.gamelab.tk/api_admin_v1/recover_password \
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
`POST /recover_password`

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
`users_by_section` | string| optional | User's company/idea section.
`start_date` | string | optional | Works with `users_by_section`. `YYYY-MM-DD` format.
`end_date` | string | optional | Works with `users_by_section`. `YYYY-MM-DD` format.