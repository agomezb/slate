---
title: API Contífico

language_tabs:
  - json : Estructura

toc_footers:
  - <a href='http://www.contifico.com'>Solicita una cuenta Contífico</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introducción

API integración Contífico

# Autenticación
```python
    curl "url_de_peticion" -H "Authorization: SECRETKEY"
```

Contífico usa una clave de acceso para sus servicios (API Key), para obtenerla debes tener una cuenta en [contífico](http://www.contifico.com) y solicitarlas por soporte al cliente. 

Esta clave será agregada y enviada como parámetro en el header `AUTHORIZATION` en cada servicio que se consuma. Un ejemplo:

`Authorization: SECRETKEY`

<aside class="notice">`SECRETKEY` debe ser reemplazado con el `api_key` proporcionado por contifico.</aside>


# Interactuando con el API
## Códigos de respuesta

CODIGO | NOMBRE | USO |
------ | ------ | --- |
`200` | Ejecución Correcta | Devuelto al ejectuar correctamente una consulta |
`201` | Objeto Creado | Devuelto al crear un objeto nuevo |
`400` | Solicitud Incorrecta | El requerimiento es incorrecto |
`404` | No Encontrado | EL objeto que se trata de consultar no existe |
`401` | No Autorizado | No se ha proporcionado las credenciales o son incorrectas |
`403` | Acceso Denegado | No se tiene permiso para entrar al recurso |
`500` | Error Interno | Error interno del sistema |

## URL's
Las siguientes son url's válidas para uso del api:

SERVICIO | URL | METODOS
------ | --- | -------
Caja | https://contifico.com/api/v1/caja/ | `GET`, `POST` |
Categoria | https://contifico.com/api/v1/categoria/ | `GET` |
Producto | https://contifico.com/api/v1/producto/ | `GET`, `POST` |
Documento | https://contifico.com/api/v1/documento/ | `GET`, `POST` |


#Caja

## Consultar
> A continuacion la respuesta al consultar una caja

```json
[
    {
        "saldo_final": "0.0",
        "total_facturado": "5.97",
        "fecha_apertura": "28/10/2015",
        "pos": "ceaa9097-1d76-4eb8-b2ba-6f412fa0297b",
        "cheque": "0.0",
        "tarjeta": "0.0",
        "efectivo": "5.97",
        "codigo": "1408",
        "fecha_cierre": "28/10/2015",
        "id": "kpKBe1JpYH6zaXyO",
        "total_cobrado": "5.97"
    },
    ...
]
```

### HTTP Request

`GET https://contifico.com/api/v1/caja/`

Devuelve un listado con todas las cajas creadas en el sistema

### HTTP Request

`GET https://contifico.com/api/v1/caja/<ID>`

Devuelve una caja con el `<ID>` solicitado.

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | varchar | 16 | Identificador de la caja en punto de venta

##Agregar

Este servicio permite agregar una nueva Caja a Contifico. La Caja es utilizada para agrupar los documentos que serán enviados y representa un turno en el punto de venta.

### HTTP Request

`POST https://contifico.com/api/v1/caja/`

```json
{
    "fecha_apertura": "28/10/2015",
    "fecha_cierre": "28/10/2015",
    "efectivo": "5.97",
    "cheque": "0.0",
    "tarjeta": "0.0",
    "total_facturado": "5.97",
    "total_cobrado": "5.97",
    "saldo_final": "0.0",
    "pos": "b1c7b7cd-36fd-1111-0000-5535b22ccfdb",
    "codigo": "838F"
}
```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
fecha_apertura | char | 10 | Fecha de Apertura de la caja
fecha_cierre | char | 10 | Fecha de Cierre de la caja
efectivo | decimal | 10,2 | Valor cobrado en Efectivo
cheque | decimal | 10,2 | Valor cobrado en Cheque
tarjeta | decimal | 10,2 | Valor cobrado en Tarjeta
total_facturado | decimal | 10,2 | Total Facturado
total_cobrado | decimal | 10,2 | Total Cobrado
saldo_final | decimal | 10,2 | Saldo por cobrar
pos | char | 36 | Punto de venta al que pertence la caja
codigo | char | 4 | Código de la caja

`El formato para fechas es dd/mm/yyyy`

<aside class="info">
La caja no es obligatoria para sincronizar documentos pero facilita el filtrado de información.
</aside>

# Documento

## Consultar
> A continuacion la respuesta al consultar un documento

```json
[
    {
        "iva": "1200.0",
        "fecha_vencimiento": "10/06/2013",
        "vendedor": null,
        "pos": "",
        "subtotal_0": "0.0",
        "descripcion": "prueba",
        "total": "11200.0",
        "id": "usdjcbjsydhcsjd",
        "servicio": null,
        "electronico": false,
        "cobros": [
            {
                "forma_cobro": "CH",
                "monto": "10000.0",
                "fecha": "01/07/2013",
                "numero_tarjeta": null,
                "numero_cheque": "201307000001",
                "tipo_ping": null
            }
        ],
        "detalles": [
            {
                "porcentaje_iva": 12,
                "cantidad": "500.0",
                "base_no_gravable": "0.0",
                "precio": "20.0",
                "porcentaje_descuento": "0.0",
                "descripcion": null,
                "producto_id": "MRYWb4kJoHz0aZ1m",
                "base_cero": "0.0",
                "base_gravable": "10000.0"
            }
        ],
        "fecha_emision": "10/06/2013",
        "documento": "221-321-321321321",
        "adicional2": "",
        "adicional1": "",
        "estado": "P",
        "tipo_documento": "FAC",
        "autorizacion": "5454654546",
        "id_caja": "",
        "subtotal_12": "10000.0",
        "cliente": {
            "tipo": "J",
            "telefonos": "2745486",
            "ruc": "0990004196001",
            "razon_social": "CORPORACION EL ROSADO S.A.",
            "direccion": "GUAYAS / GUAYAQUIL / AV. 9 DE OCTUBRE 729 Y BOYACA - GARCIA AVILES",
            "es_extranjero": false,
            "porcentaje_descuento": null,
            "es_cliente": false,
            "id": null,
            "es_empleado": false,
            "email": "agomez@contifico.com",
            "cedula": "",
            "es_vendedor": false
        }
    },
    ...
]
```

### HTTP Request

`GET https://contifico.com/api/v1/caja/<ID>`

Devuelve un documento con el `<ID>` solicitado.


Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | varchar | 16 | Identificador de la caja en punto de venta

<hr>

Contenido | Representacion |
--------- | -------------- |
"cobros": [{ .. }] | Contiene los cobros realizados en el documento.  |
"detalles": [{ .. }] | Contiene los detalles del documento, bien/servicio agregados al documento.  |
"cliente": { .. } | El cliente que esta asignado al documento. |


## Agregar

```json
{
    "pos" : "ceaa9097-1d76-4eb8-0000-6f412fa0297b",
    "fecha_emision": "01/11/2015",
    "tipo_documento": "FAC",
    "documento": "001-001-000008089",
    "estado": "P",
    "autorizacion": "0123456789",
    "caja_id": null,
    "cliente": {
        "ruc": "0922054366001",
        "cedula": "0922054366",
        "razon_social": "Nombres del Cliente",
        "telefonos": "0988800001",
        "direccion": "Direccion cliente",
        "tipo": "N",
        "email": "cliente@contifico.com",
        "es_extranjero": false
    },
    "vendedor": {
        "ruc": "0904728680001",
        "cedula": "0904728680",
        "razon_social": "Nombres del Vendedor",
        "telefonos": "5104910",
        "direccion": "direccion del vendedor",
        "tipo": "N",
        "email": "vendedor@contifico.com",
        "es_extranjero": false
    },
    "descripcion": "FACTURA 8040",
    "subtotal_0": 0.00,
    "subtotal_12": 1.35,
    "iva": 0.16,
    "servicio": 0.00,
    "total": 1.51,
    "adicional1": "",
    "adicional2": "",
    "detalles": [{
        "producto_id": "RZxg87rxLh9Mb1pV",
        "cantidad": 1.00,
        "precio": 1.00,
        "porcentaje_iva": 12,
        "porcentaje_descuento": 0.00,
        "base_cero": 0.00,
        "base_gravable": 1.00,
        "base_no_gravable": 0.00
    },
    {
        "producto_id": "YqxgeprxLh9981cU",
        "cantidad": 1.00,
        "precio": 0.35,
        "porcentaje_iva": 12,
        "porcentaje_descuento": 0.00,
        "base_cero": 0.00,
        "base_gravable": 0.35,
        "base_no_gravable": 0.00
    }],
    "cobros":[{
      "forma_cobro" : "TC",
      "monto" : 1.51,
      "numero_cheque" : "4567897",
      "tipo_ping" : "D"
    }]
}

```

Este servicio permite agregar un nuevo documento a Contifico.

### HTTP Request

`POST https://contifico.com/api/documento/`

### Query Parameters

Contenido | Representacion |
--------- | -------------- |
"cliente": { .. } | El cliente que esta asignado al documento. Si no existe, lo crea. |
"vendedor": { .. } | El vendedor que esta asignado al documento. Si no existe, lo crea. |
"detalles": [{ .. }] | Contiene los detalles del documento, bien/servicio agregados al documento.  |
"cobros": [{ .. }] | Contiene los cobros realizados en el documento.  |

# Producto

## Consultar
> A continuacion la respuesta al consultar productos

```json
[
    {
        "codigo_barra": "000000011114",
        "porcentaje_iva": 12,
        "categoria_id": "5onPeR2nBil9ep1v",
        "minimo": "3.0",
        "pvp2": "250.0",
        "pvp3": "300.0",
        "pvp1": "200.0",
        "pvp_manual": false,
        "descripcion": "COMPUTADORA 11\"",
        "nombre": "LAPTOP DELL SRS PREMIUM SOUND",
        "codigo": "00001",
        "estado": "A",
        "id": "lY4erkB4vtrVa2Lz",
        "cantidad_stock": "-5.0"
    },
    ...
]

```
### HTTP Request

`GET https://contifico.com/api/v1/producto/`

Devuelve un listado con todos los productos creados en el sistema

### HTTP Request

`GET https://contifico.com/api/v1/producto/<ID>`

Devuelve un producto con el `<ID>` solicitado.


Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | varchar | 16 | Identificador del producto en el sistema.

## Agregar
> A continuacion el formato de envio para crear un producto

```json
    {
        "codigo_barra": "000000011115",
        "porcentaje_iva": "12",
        "categoria_id": "xgArb6B7wsPObyR4",
        "minimo": "3.0",
        "pvp2": "250.0",
        "pvp3": "300.0",
        "pvp1": "200.0",
        "pvp_manual": false,
        "descripcion": "COMPUTADORA 11\"",
        "nombre": "LAPTOP DELL SRS PREMIUM SOUND 3333",
        "codigo": "00024",
        "estado": "A"
    }
```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
categoria_id | varchar | 16 | Identificador de la categoria en el sistema.
