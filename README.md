# API - Clami DTE

Clami DTE, es un servicio de facturación electrónica que mediante la integración de nuestro webservice
o uso del portal web de facturación, podrá realizar la emisión de documentos electrónicos tributarios

### Tabla de contenidos
**[Configuración Header](#configuración-header)**  
**[Envío en formato json](#envío-en-formato-json)**  
**[Envío en formato csv](#envío-en-formato-csv)**  
**[Respuestas a envío](#respuestas-a-envío)**  

### Introduccion

Para poder enviar un documento mediante webservice, se requiere que los datos que lo representan se encuentren
en formato JSON o CSV dependiendo de su preferencia, una vez formateado el documento se debe enviar mediante un HTTP Request vía POST. El contenido del request debe ser de tipo JSON y se debe incluir un token de autorización que identifique su usuario, el que debe ser obtenido del portal de facturación. Una vez realizado el envío, el servidor retornara la respuesta en formato JSON.

### Configuración Header

Se requiere que el header especifique el tipo de contenido JSON y el Token de autorización.

```js
Content-Type: application/json
Authorization: Token <value_token>
```

## Envío en formato json

### URL Envio:

Url utilizada para realizar envio de documentos en formato JSON al webservice de Clami DTE.

```
POST /v2/enviar/dte
```

### Ejemplo envio vía curl:

Ejemplo de envio de un documento en formato JSON, via CURL.

* @dte.json : Corresponde al documento envíado

```
curl -H "Authorization:Token <value_token>" -H "Content-Type: application/json" --data @dte.json
http://clami.cl/v2/enviar/dte
```

### Plantilla General

La siguiente plantilla es el template general en formato JSON, utilizada por Clami DTE para la formacion y envio de los siguientes documentos:

+ Factura Electrónica
+ Factura de compra
+ Nota de credito
+ Nota de debito
+ Guia de despacho

```js
{
  "Caratula": {
    "RutEmisor": "value",
    "RutEnvia": "value",
    "RutReceptor": "value",
    "FchResol": "value",
    "NroResol": "value"
  },
  "Encabezado": {
    "IdDoc": {
      "TipoDTE": "value",
      "Folio": "value",
      "FchEmis": "value",
      "IndNoRebaja": "value",
      "TipoDespacho": "value",
      "IndTraslado": "value",
      "TpoImpresion": "value",
      "IndServicio": "value",
      "MntBruto": "value",
      "FmaPago": "value",
      "FmaPagExp": "value",
      "MntPagos": {
        "FchPago": "value",
        "MntPago": "value",
        "GlosaPagos": "value"
      },
      "PeriodoDesde": "value",
      "PeriodoHasta": "value",
      "MedioPago": "value",
      "TpoCtaPago": "value",
      "NumCtaPago": "value",
      "BcoPago": "value",
      "TermPagoCdg": "value",
      "TermPagoGlosa": "value",
      "TermPagoDias": "value",
      "FchVenc": "value"
    },
    "Emisor": {
      "RUTEmisor": "value",
      "RznSoc": "value",
      "GiroEmis": "value",
      "Telefono": "value",
      "CorreoEmisor": "value",
      "Acteco": "value",
      "DirOrigen": "value",
      "CmnaOrigen": "value",
      "CiudadOrigen": "value",
      "CdgVendedor": "value",
    },
   "Receptor": {
      "RUTRecep": "value",
      "RznSocRecep": "value",
      "GiroRecep": "value",
      "Contacto": "value",
      "CorreoRecep": "value",
      "DirRecep": "value",
      "CmnaRecep": "value",
      "CiudadRecep": "value",
      "DirPostal": "value",
      "CmnaPostal": "value",
      "CiudadPostal": "value"
    },
    "RUTSolicita": "value",
    "Transporte": {
      "Patente": "value",
      "RUTTrans": "value",
      "Chofer": {
        "RUTChofer": "value",
        "NombreChofer": "value"
      },
      "DirDest": "value",
      "CmnaDest": "value",
      "CiudadDest": "value"
    },
  },
  "Detalle": [{
    "CdgItem": {
      "TpoCodigo": "value",
      "VlrCodigo": "value"
    },
    "IndExe": "value",
    "NmbItem": "value",
    "DscItem": "value",
    "QtyRef": "value",
    "UnmdRef": "value",
    "PrcRef": "value",
    "QtyItem": "value",
    "FchElabor": "value",
    "FchVencim": "value",
    "UnmdItem": "value",
    "PrcItem": "value",
    "DescuentoPct": "value",
    "DescuentoMonto": "value",
    "RecargoPct": "value",
    "RecargoMonto": "value",
    "CodImpAdic": "value",
    "MontoItem": "value"
  }],
   "DscRcgGlobal": [{
    "TpoMov": "value",
    "GlosaDR": "value",
    "TpoValor": "value",
    "ValorDR": "value",
    "IndExeDR": "value"
  }],
    "Referencia": [{
    "NroLinRef": "value",
    "TpoDocRef": "value",
    "IndGlobal": "value",
    "FolioRef": "value",
    "RUTOtr": "value",
    "FchRef": "value",
    "CodRef": "value",
    "RazonRef": "value"
  }]
}
```
###Ejemplo Factura Electrónica

A continuación se da a conocer un ejemplo de factura electrónica en formato JSON.

```js

{
  "Caratula": {
    "RutEmisor": "1111111-1",
    "TmstFirmaEnv": "R",
    "RutReceptor": "60803000-K",
    "RutEnvia": "2222222-2",
    "NroResol": "0",
    "FchResol": "2014-03-04"
  },
  "Documentos": [
    {
      "Encabezado": {
        "Totales": {
          "TasaIVA": "19"
        },
        "IdDoc": {
          "TipoDTE": "33",
          "FchEmis": "2016-05-25"
        },
        "Emisor": {
          "RUTEmisor": "1111111-1",
          "CiudadOrigen": "SANTIAGO",
          "Acteco": "726000",
          "GiroEmis": "SERVICIOS INTEGRALES DE INFORMATICA",
          "CmnaOrigen": "SAN BERNARDO",
          "RznSoc": "EMPRESA PRUEBA",
          "DirOrigen": "LINGUE 4789"
        },
        "Receptor": {
          "RznSocRecep": "JORGE GONZALEZ LTDA",
          "CmnaRecep": "LA FLORIDA",
          "DirRecep": "SAN DIEGO 2222",
          "GiroRecep": "COMPUTACION",
          "RUTRecep": "3333333-3",
          "CiudadRecep": "SANTIAGO"
        }
      },
      "Referencia": [
        {
          "TpoDocRef": "SET",
          "FchRef": "2016-05-25",
          "FolioRef": "72",
          "NroLinRef": "1",
          "RazonRef": "CASO 478979-3",
	  "CodRef":"1"
        }
      ],
      "Detalle": [
        {
          "PrcItem": "4827",
          "NmbItem": "Pintura B&W AFECTO",
          "NroLinDet": "1",
          "QtyItem": "40"
        },
        {
          "PrcItem": "3475",
          "NmbItem": "ITEM 2 AFECTO",
          "NroLinDet": "2",
          "QtyItem": "198"
        },
        {
          "QtyItem": "1",
          "PrcItem": "35033",
          "NmbItem": "ITEM 3 SERVICIO EXENTO",
          "NroLinDet": "3",
          "IndExe": "1"
        }
      ]
    }
  ]
}
```

## Envío en formato csv

### URL Envio:
```
POST /v1/enviar/dte
```

###Plantilla General

La siguiente plantilla es el template general en formato CSV, utilizada por Clami DTE para la formacion y envio de los siguientes documentos:

+ Factura Electrónica
+ Factura de compra
+ Nota de credito
+ Nota de debito
+ Guia de despacho

```java
CA
Caratula;"RutEmisor";"RutEnvia";"RutReceptor";"FchResol";"NroResol";"TmstFirmaEnv";"SubTotDTE"
EN
Encabezado;"R";"R";"RUTMandante";"R";"RUTSolicita";"R";"R";"OtraMoneda"
IdDoc;"TipoDTE";"Folio";"FchEmis";"IndNoRebaja";"TipoDespacho";"IndTraslado";"TpoImpresion";"IndServicio";"MntBruto";"FmaPago";"FmaPagExp";"FchCancel";"MntCancel";"SaldoInsol";"MntPagos";"PeriodoDesde";"PeriodoHasta";"MedioPago";"TpoCtaPago";"NumCtaPago";"BcoPago";"TermPagoCdg";"TermPagoGlosa";"TermPagoDias";"FchVenc"
MntPagos;"FchPago";"MntPago";"GlosaPagos"
Emisor;"RUTEmisor";"RznSoc";"GiroEmis";"Telefono";"CorreoEmisor";"Acteco";"R";"Sucursal";"CdgSIISucur";"DirOrigen";"CmnaOrigen";"CiudadOrigen";"CdgVendedor";"IdAdicEmisor"
Receptor;"RUTRecep";"CdgIntRecep";"RznSocRecep";"Extranjero";"GiroRecep";"Contacto";"CorreoRecep";"DirRecep";"CmnaRecep";"CiudadRecep";"DirPostal";"CmnaPostal";"CiudadPostal"
Transporte;"Patente";"RUTTrans";"R";"DirDest";"CmnaDest";"CiudadDest";"R"
Chofer;"RUTChofer";"NombreChofer"
TipoBultos;"CodTpoBultos";"CantBultos";"Marcas";"IdContainer";"Sello";"EmisorSello"
ImptoReten;"TipoImp";"TasaImp";"MontoImp" (repetible)
DE
Detalle;"NroLinDet";"R";"IndExe";"Retenedor";"NmbItem";"DscItem";"QtyRef";"UnmdRef";"PrcRef";"QtyItem";"Subcantidad";"FchElabor";"FchVencim";"UnmdItem";"PrcItem";"OtrMnda";"DescuentoPct";"DescuentoMonto";"SubDscto";"RecargoPct";"RecargoMonto";"SubRecargo";"CodImpAdic";"MontoItem"
#Campo repetible 
CdgItem;"TpoCodigo";"VlrCodigo"
DR
#Campo repetible
DscRcgGlobal;"NroLinDR";"TpoMov";"GlosaDR";"TpoValor";"ValorDR";"ValorDROtrMnda";"IndExeDR"
RE
#Campo repetible
Referencia;"NroLinRef";"TpoDocRef";"IndGlobal";"FolioRef";"RUTOtr";"FchRef";"CodRef";"RazonRef"
```

### Ejemplo CSV

A continuación se muestra el mismo ejemplo que se dio a conocer para el formato JSON, pero en esta ocasión en formato CSV.


```ruby
CA
Caratula;1111111-1;2222222-2;60803000-K;2014-03-04;0;R;
EN
Totales;;;;;19;;;;;;;;;;;;;
IdDoc;33;;2016-05-25;;;;;;;;;;;;;;;;;;;;;;
Emisor;1111111-1;EMPRESA PRUEBA;SERVICIOS INTEGRALES DE INFORMATICA;;;726000;;;;LINGUE 4789;SAN BERNARDO;SANTIAGO;;
Receptor;3333333-3;;JORGE GONZALEZ LTDA;;COMPUTACION;;;SAN DIEGO 2222;LA FLORIDA;SANTIAGO;;;
Encabezado;;;;;;;;
DE1
Detalle;1;;;;Pintura B&W AFECTO;;;;;40;;;;;4827;;;;;;;;;;
DE2
Detalle;2;;;;ITEM 2 AFECTO;;;;;198;;;;;3475;;;;;;;;;;
DE3
Detalle;3;;1;;ITEM 3 SERVICIO EXENTO;;;;;1;;;;;35033;;;;;;;;;;
RE1
Referencia;1;SET;;72;;2016-05-25;1;CASO 478979-3
FIN
```


```js
{
"Documento":"Q0EKQ2FyYXR1bGE7MTsyOzM7NDs1OzY7NwpFTgp\
FbmNhYmV6YWRvOzE7MjszOzQ7NTs2Ozc7OApJZERvYzsxOzI7Mzs\
0OzU7Njs3Ozg7OTsxMDsxMTsxMjsxMzsxNDsxNTsxNjsxNzsxODs\
xOTsyMDsyMTsyMjsyMzsyNDsyNQpNbnRQYWdvczsxOzI7MwpFbWl\
zb3I7MTsyOzM7NDs1OzY7Nzs4Ozk7MTA7MTE7MTI7MTM7MTQKUmV\
jZXB0b3I7MTsyOzM7NDs1OzY7Nzs4Ozk7MTA7MTE7MTI7MTMKVHJ\
hbnNwb3J0ZTsxOzI7Mzs0OzU7Njs3CkNob2ZlcjsxOzIKSW1wdG9\
SZXRlbjsxOzI7MwpERQpEZXRhbGxlOzE7MjszOzQ7NTs2Ozc7ODs\
5OzEwOzExOzEyOzEzOzE0OzE1OzE2OzE3OzE4OzE5OzIwOzIxOzI\
yOzIzOzI0OzI1CkNkZ0l0ZW07MTsyClN1YkRzY3RvOzE7MgpTdWJ\
SZWNhcmdvOzE7MgpEUgpEc2NSY2dHbG9iYWw7MTsyOzM7NDs1OzY\
7NwpSRQpSZWZlcmVuY2lhOzE7MjszOzQ7NTs2Ozc7OA="
}
```


##Respuestas a envío

A continuación se da a conocer los tipos de respuestas posibles retornadas por el servidor las que se encuentran en formato JSON.

**TIpos de campos**:

codigo  | estado |  detalle     | documentos
--------|--------|--------------|------------
**200**     | OK     | NO | SI
**600**    | Firma digital no cargada | NO | NO
**601**    | Error validación Schema DTE | SI | NO
**602**    | Error validación Schema Envio | SI | NO
**603**     | Error Firmado | NO | NO
**604**     | Error al normalizar datos | SI | NO
**605**     | Caracter no soportado para ISO-8859-1 en el campo **\<nombre_campo>** | NO | NO
**500**    | Error servidor | NO | NO
**501**     | Falta campo para normalizar el documento | SI | NO
**502**     | Rut Emisor incorrecto para este usuario | NO | NO
**503**     | Rut Envía incorrecto para este usuario | NO | NO
**504**     | El campo **\<nombre_campo>**, no corresponde o no se encuentra en la sección correcta | NO | NO
**505**     | El documento se encuentra mal codificado (Base64) | NO | NO
**506**     | El Folio **\<numero_folio>** del tipo DTE **\<tipo_dte>**, ya fue utilizado | NO | NO
**507**     | No hay folios disponibles, para el tipo de dte **\<tipo_dte>** | NO | NO
**508**     | No se encontro CAF para el folio **\<numero_folio>**, tipo DTE **\<tipo_dte>**  | NO | NO
**509**     | El documento csv, no se encuentra en base64 | NO | NO
**404**     | No se pudo resolver el host | NO | NO
**405**     | Error desconocido, al enviar el documento | NO | NO
**406**     | No se pudo obtener un token de autenticación con SII| NO | NO


Para los casos de exito se incluye el campo 'documentos', que contiene los datos de los documentos enviados y generados.

#####Campos incluidos en 'documentos':

* **xml**: URL temporal que contiene el xml del documento, en caso de q se desee obtener para impresión u otro objetivo
* **pdf**: URL temporal que contiene el xml del documento, en caso de q se desee obtener para impresión u otro objetivo
* **folio**: Folio asignado al documento
* **tipo**: Tipo de documento


##### Ejemplo respuesta en caso de exito

```js
{  
   "codigo":200,
   "estado":"OK",
   "documentos":[  
      {  
         "xml":"https://documentos-tributarios.s3.amazonaws.com/78961710-4/DTE/T33F191.xml?Signature=P4ctCMM4RkFyX3FMeTszMRgDpeY%3D&Expires=1465660393&AWSAccessKeyId=AKIAIJ7B6LV6CKCSFD7Q",
         "pdf":"https://documentos-tributarios.s3.amazonaws.com/78961710-4/PDF/33/T33F191.pdf?Signature=IQnSV7tZY%2FDNyGKtFttGDLp0SYg%3D&Expires=1465660393&AWSAccessKeyId=AKIAIJ7B6LV6CKCSFD7Q",
         "folio":"191",
         "tipo":"33"
      }
   ]
}
```
