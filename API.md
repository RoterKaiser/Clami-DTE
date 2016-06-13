# API


### Configuración Header

Se requiere que el header especifique el tipo de contenido JSON y el Token de autorización.

```
Content-Type: application/json
Authorization: Token <value_token>
```

## Metodos de envío en formato JSON

### URL Envio:

Url utilizada para realizar envio de documentos en formato JSON al webservice de Clami DTE.

```
POST /v2/enviar/dte
```

### Ejemplo envio vía curl:

Ejemplo de envio de un documento en formato JSON, via CURL

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

```
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
    "CodImpAdic": "value"
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

Archivo **dte.sjon:**

```

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



