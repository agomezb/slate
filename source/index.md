---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introdución

API Sincronización documento, productos y personas Contífico

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Facturas

## Agregar Nuevo Documento

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> Esta petición devuelve un JSON como:

```json
{
    "result": true
}
```

Este servicio permite agregar un nuevo documento a Contifico.

### HTTP Request

`POST https://contifico.com/api/documento`

### Query Parameters

Parámetro | Default | Descripción
--------- | ------- | -----------
id | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember a happy kitten is an authenticated kitten!
</aside>


### Caja

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | string | 10 | Identificador de la caja en punto de venta
fecha_apertura | string | 11 | Fecha de Apertura de la caja
fecha_cierre | string | 11 | Fecha de Cierre de la caja
monto_inicial | decimal | 13,5 | Monto de Apertura
total_cierre | decimal | 13,5 | Total Cobrado al cierre
efectivo | decimal | 13,5 | Valor cobrado en Efectivo
cheque | decimal | 13,5 | Valor cobrado en Cheque
tarjeta | decimal | 13,5 | Valor cobrado en Tarjeta
total_facturado | decimal | 13,5 | Total Facturado
total_cobrado | decimal | 13,5 | Total Cobrado
saldo_final | decimal | 13,5 | Saldo por cobrar

<aside class="warning">
La caja no es obligatoria para sincronizar documentos pero si necesaria para reportería
</aside>


## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

