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

Contífico usa dos claves de acceso para sus servicios (API Key y API Token), para obtenerlas debes tener una cuenta en [contífico](http://www.contifico.com) y solicitarlas por soporte al cliente. 

Estan claves serán agregadas y enviadas como parámetros en cada servicio que se consuma.

Ejemplo claves:

`api_key: GUK8mS5AVzyPZ1QCbKNAFVLvp9n3egWug2HyrfKPOII`

`api_token: b5c5b5cd-36fd-4494-9589-9935b22ccfdb`

#Caja

##Agregar

Este servicio permite agregar una nueva Caja a Contifico. La Caja es utilizada para agrupar los documentos que serán enviados y representa un turno en el punto de venta.

### HTTP Request

`POST https://contifico.com/api/caja/`

### Caja

```json
{
   	"api_key" : "GUK8mS5AVzyPZ1QCbKNAFVLvp9n3egWug2HyrfKPOII",
	"api_token" : "b5c5b5cd-36fd-4494-9589-9935b22ccfdb",
	"caja" :
	{
    	"id": "33",
    	"fecha_apertura": "2015-09-07",
    	"fecha_cierre": "2015-09-10",
    	"efectivo": 5.97,
    	"cheque": 0.00,
    	"tarjeta": 0.00,
    	"total_facturado": 5.97,
    	"total_cobrado": 5.97,
    	"saldo_final": 0.00
	}
}
```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | varchar | 11 | Identificador de la caja en punto de venta
fecha_apertura | char | 10 | Fecha de Apertura de la caja
fecha_cierre | char | 10 | Fecha de Cierre de la caja
efectivo | decimal | 10,2 | Valor cobrado en Efectivo
cheque | decimal | 10,2 | Valor cobrado en Cheque
tarjeta | decimal | 10,2 | Valor cobrado en Tarjeta
total_facturado | decimal | 10,2 | Total Facturado
total_cobrado | decimal | 10,2 | Total Cobrado
saldo_final | decimal | 10,2 | Saldo por cobrar

`El formato para fechas es yyyy-mm-dd`

<aside class="info">
La caja no es obligatoria para sincronizar documentos pero facilita el filtrado de información.
</aside>

# Documento

## Agregar

Este servicio permite agregar un nuevo documento a Contifico.

### HTTP Request

`POST https://contifico.com/api/documento/`

### Query Parameters

```json
{
    "api_key": "GUK8mS7AVzyPZ1QCbKNAFVLvp8n3egWug2HyrfKPOII",
    "api_token": "b1c7b7cd-36fd-4494-9589-5535b22ccfdb",
    "personas" : [],
    "documento": {}
}

```
> Los atributos personas y documento son explicados a continuación

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
api_key | char | - | Api Key Sincronización
api_token | char | - | Api Token Sincronización
personas | Lista Personas | - | Listado de personas involucradas en el documento
documento | Objeto Documento  | - | Datos del Documento

## Personas

```json

[{
    "ruc": "",
    "cedula": "0922054366",
    "razon_social": "Nombre Vendedor",
    "telefonos" : "5104910",
    "direccion": "direccion persona",
    "tipo": "N",
    "email": "info@contifico.com",
    "es_vendedor": true,
    "es_extranjero": false,
    "tipo_contrato" : "A",
    "sueldo" : 400
},
{
    "ruc": "0904728680001",
    "cedula": "0904728680",
    "razon_social": "Razon Social Cliente",
    "telefonos" : "0988845001",
    "direccion": "Direccion cliente",
    "tipo": "N",
    "email": "cliente@contifico.com",
    "es_vendedor": false,
    "es_extranjero": false,
    "tipo_contrato" : "",
    "sueldo" : ""
}]

```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
ruc | char | 13 | Ruc de la persona
cedula | char | 10 | Cédula de la persona
razon_social | varchar | 300 | Razón social o nombre de la persona
telefonos | varchar | 100 | Teléfonos de la persona
direccion | varchar | 300 | Dirección de la persona
tipo | char | 1 | Tipo de Persona N: Natural, J: Jurídica
email | varchar | 75 | Email de la persona
es_extranjero | boolean | - | Identifica si una persona es extranjera
es_vendedor | boolean | - | Identifica si una persona es vendedor
tipo_contrato | char | 1 | Tipo de contrato del empleado. A: Afiliado, S: Serivicios Prestados
sueldo | decimal | 10,2 | Sueldo del empleado

<aside class="notice">
Sólo es necesario enviar una sóla vez los datos de cada persona a contífico, en proximos envios bastará con especificar el RUC o Cédula en los datos del documento.
</aside>

## Documento

```json

{
        "id" : "80",
        "fecha_emision": "2015-09-24",
        "tipo_documento": "FAC",
        "documento": "001-001-000008040",
        "estado": "C",
        "autorizacion": "",
        "id_caja" : "1",
        "cliente": "0922054366",
        "vendedor": "0904728680001",
        "descripcion": "FACTURA 8040",
        "subtotal_0": 0.00,
        "subtotal_12": 1.35,
        "iva": 0.16,
        "servicio": 0.00,
        "total": 1.51,
        "adicional1": "",
        "adicional2": "",
        "detalles": [],
        "cobros": []
}

```

> Los atributos detalles y cobros son explicados posteriormente


Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id | varchar | 11 | Identificador del documento en punto de venta
fecha_emision | char | 10 | Fecha de emisión de la factura
tipo_documento | char | 3 | Tipo de Documento. FAC: Factura, NVE: Nota de Venta
documento | char | 17 | Número de documento. Ej: 001-001-000000123
estado | char | 1 | Estado de la Factura. P: Pendiente, C: Cobrada, A: Anulada
autorizacion | varchar | 37 | Número de autorización de la factura
id_caja | varchar | 11 | Identificador de la caja
cliente | varchar | 11 | Identificador del cliente RUC o Cédula
vendedor | varchar | 11 | Identificador del Vendedor RUC o Cédula
descripcion | varchar | 300 | Descripción de la factura
subtotal_0 | decimal | 13,5 | Base Cero de la factura
subtotal_12 | decimal | 13,5 | Base Gravable de la factura
iva | decimal | 13,5 | Valor del IVA en la factura
servicio | decimal | 13,5 | Valor del servicio en la factura
total | decimal | 13,5 | Valor total de la factura
adicional1 | varchar | 300 | Campo libre
adicional2 | varchar | 300 | Campo libre
detalles | Lista Detalle | - | Listado de productos/servicios
cobros | Lista Cobro  | - | Listado de cobros

### Detalles (Productos/Servicios)

```json

[{
    "id_contifico": 10449,
    "cantidad": 1.00,
    "precio": 1.00,
    "porcentaje_iva": 12,
    "porcentaje_descuento": 0.00,
    "base_cero": 0.00,
    "base_gravable": 1.00,
    "base_no_gravable": 0.00
},
{
    "id_contifico": 10450,
    "cantidad": 1.00,
    "precio": 0.35,
    "porcentaje_iva": 12,
    "porcentaje_descuento": 0.00,
    "base_cero": 0.00,
    "base_gravable": 0.35,
    "base_no_gravable": 0.00
}]

```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
id_contifico | int | 11 | Identificador del producto o Servicio en Contifico
cantidad | decimal | 10,2 | Cantidad de items vendidos
precio | decimal | 13,5 | Precio unitario del item
porcentaje_iva | INT | 2 | Porcentaje IVA que se aplica al item: 0 o 12
porcentaje_descuento | decimal | 5,2 | Porcentaje de descuento en el item
base_cero | decimal | 10,2 | Valor de base 0% para cada item
base_gravable | decimal | 10,2 | Valor de base 12% para cada item
base_no_gravable | decimal | 10,2 | Valor de la base no gravable

### Cobros

```json

[{
    "forma_cobro": "EF",
    "monto": 1.51,
    "numero_cheque": "",
    "tipo_ping": ""
}]

```

Parámetro | Tipo | Longitud | Descripción |
--------- | ---- | -------- | ----------- |
forma_cobro | char | 2 | Forma de Cobro del documento: EF: Efectivo, TC: Tarjeta de Crédito, CH: Cheque
monto | varchar | 3 | Valor del cobro
numero_cheque | varchar | 15 | Número de cheque
tipo_ping | char | 1 | Tipo de red usada: D: Datafast, M: Medianet, R: Red de Apoyo

##Estructura Completa

### HTTP Request

```json

{
    "api_key": "GUK8mS7AVzyPZ1QCbKNPFVLvp8n3ehWug2HyrfKPOII",
    "api_token": "b1c7b7cd-36fd-4494-9589-5535b99oofdb",
    "personas" : [{
        "ruc": "",
        "cedula": "0922054366",
        "razon_social": "Nombre Vendedor",
        "telefonos" : "5104910",
        "direccion": "direccion persona",
        "tipo": "N",
        "email": "info@contifico.com",
        "es_vendedor": true,
        "es_extranjero": false,
        "tipo_contrato" : "A",
        "sueldo" : 400
    },
    {
        "ruc": "0904728680001",
        "cedula": "0904728680",
        "razon_social": "Razon Social Cliente",
        "telefonos" : "0988845001",
        "direccion": "Direccion cliente",
        "tipo": "N",
        "email": "cliente@contifico.com",
        "es_vendedor": false,
        "es_extranjero": false,
        "tipo_contrato" : "",
        "sueldo" : ""
    }],
    "documento": {
        "id" : "80",
        "fecha_emision": "2015-09-24",
        "tipo_documento": "FAC",
        "documento": "001-001-000008040",
        "estado": "C",
        "autorizacion": "0123456789",
        "id_caja" : "1",
        "cliente": "0904728680001",
        "vendedor": "0922054366",
        "descripcion": "FACTURA 8040",
        "subtotal_0": 0.00,
        "subtotal_12": 1.35,
        "iva": 0.16,
        "servicio": 0.00,
        "total": 1.51,
        "adicional1": "",
        "adicional2": "",
        "detalles": [{
            "id_contifico": 10449,
            "cantidad": 1.00,
            "precio": 1.00,
            "porcentaje_iva": 12,
            "porcentaje_descuento": 0.00,
            "base_cero": 0.00,
            "base_gravable": 1.00,
            "base_no_gravable": 0.00
        },
        {
            "id_contifico": 10450,
            "cantidad": 1.00,
            "precio": 0.35,
            "porcentaje_iva": 12,
            "porcentaje_descuento": 0.00,
            "base_cero": 0.00,
            "base_gravable": 0.35,
            "base_no_gravable": 0.00
        }],
        "cobros": [{
            "forma_cobro": "EF",
            "monto": 1.51,
            "numero_cheque": "",
            "tipo_ping": ""
        }]
    }
}

```

`POST https://contifico.com/api/documento/`