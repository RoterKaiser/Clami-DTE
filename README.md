# API - Clami DTE

Clami DTE, es un servicio SaaS de facturación electrónica para Chile que ofrece la posibilidad de integración para
emitir Facturas, Notas de Crédito, Guias y Boleta electrónicas.

## Tabla de contenidos
**[Configuración Header](#configuración-header)**  
**[Ejemplo vía curl](#ejemplo-vía-curl)**   
**[Respuestas a la solicitud POST](#respuestas-a-la-solicitud-post)**  

## Introducción

Para poder generar un documento electrónico mediante Webservice, se requiere que los datos que lo representan se encuentren
en formato JSON o CSV dependiendo de su preferencia, y se debe enviar mediante una solicitud HTTP Request con el metodo POST. El contenido del request debe ser de tipo JSON y se debe incluir un token de autorización en el encabezado que identifique su usuario ( Este debe ser obtenido del portal de facturación ). Una vez realizado el envío, el servidor retornara la respuesta en formato JSON.

## Configuración Header

Se requiere que el header especifique el tipo de contenido JSON y el Token de autorización.

```js
Content-Type: application/json
Authorization: Token <value_token>
```

## URL - POST Request

Para documentos electrónicos que no sean boleta:
```
POST /v2/enviar/dte
```
Para boletas electrónicas:
```
POST /v2/enviar/boleta
```

## Ejemplo vía curl:

Ejemplo de envió de un documento en formato JSON, vía CURL.

* @dte.json : Corresponde al documento envíado

```
curl -H "Authorization:Token <value_token>" -H "Content-Type: application/json" --data @dte.json
http://clami.cl/v2/enviar/dte
```


### JSON Boleta Electrónica

A continuación se da a conocer un ejemplo de factura electrónica en formato JSON.

```js
{
   "Documentos":[
      {
         "Encabezado":{
            "IdDoc":{
               "IndServicio":3,
               "TipoDTE":"39"
            },
            "Receptor":{
               "RznSocRecep":"INDEFINIDO",
               "RUTRecep":"66666666-6"
            }
         },
          // En esta sección se pueden agregar los medios de pago y vuelto, para que salgan en el PDF
         "SubTotInfo":[
            {
               "GlosaSTI":"TARJETA DEBITO",
               "ValSubtotSTI":"10000",
            },
            {
               "GlosaSTI":"EFECTIVO",
               "ValSubtotSTI":"5000",
            },
            {
               "GlosaSTI":"VUELTO",
               "ValSubtotSTI":390,
            }
         ],
          // En esta sección se pueden agregar los datos de la caja
          // Se debe respertar la glosa "DATOS CAJA" para que sea reconocido el campo
          // Si el código de vendedor existe en Clami, se desglozara su nombre en la boleta.
         "Referencia":[
            {
               "CodVndor":1,
               "CodCaja":1,
               "RazonRef":"DATOS CAJA"
            }
         ],
          // Sección de Descuentos y Recargos
          // TpoMov:D , para esepecificar descuento
          // TpoMov:R , para esepecificar recargo
          // La GlosaDR, se imprime en la boleta
         "DscRcgGlobal":[
            {
               "TpoMov":"D",
               "ValorDR":3653,
                 // Glosa obligatoria para especificar descuento global
               "GlosaDR":"Descuento",
               "TpoValor":"$"
            },
            {
               "TpoMov":"D",
               "ValorDR":3, 
                // Glosa obligatoria para especificar ley del redondeo
               "GlosaDR":"LeyDelRedondeo",
               "TpoValor":"$"
            }
         ],
          // Sección para el detalle de la boleta ( productos )
         "Detalle":[
            {
               "CdgItem":{
                  "VlrCodigo":"9937993755000",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"ALMOHADILLA TERAP.CHICA",
               "QtyItem":5,
               "PrcItem":2790
            },
            {
               "CdgItem":{
                  "VlrCodigo":"9980808776653",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"SAL INGLESA 30GR.",
               "QtyItem":3,
               "PrcItem":690
            },
            {
               "UnmdItem":"UND",
               "PrcItem":1248,
               "CdgItem":{
                  "VlrCodigo":"99176480372",
                  "TpoCodigo":"INT1"
               },
               "NmbItem":"COLGATE TOT12 GEL WH.97GR",
               "DescuentoMonto":250, // Ejemplo de descuento
               "QtyItem":2
            }
         ]
      }
   ]
}
```


### JSON Nota de crédito - Aulacion parcial - Boleta

A continuación se da a conocer un ejemplo de Nota de crédito por anulación parcial

```js
{
   "Documentos":[
      {
         "Encabezado":{
            "IdDoc":{
               "TipoDTE":"61"
            },
            "Receptor":{
               "RznSocRecep":"INDEFINIDO",
               "RUTRecep":"66666666-6"
            }
         },
         "Referencia":[
            {
                // Anulación a boleta 
               "TpoDocRef":"39",
               "FchRef":"2021-03-31",
               "RazonRef":"Anulación documento",
               "FolioRef":"2590",
                // Este campo indica el tipo de nota de crédito
                // 1: Anula Documento de referencia (Anulacion total los montos de la NC === Factura o Boleta)
                // 2: Corrige Texto Documento de referencia
                // 3: Corrige montos ( Anulacion Parcial, los montos de la NC !== Factura o Boleta )
               "CodRef":"1"
            }
         ],
         "DscRcgGlobal":[
            {
               "TpoMov":"D",
               "ValorDR":6,
                // Glosa obligatoria para especificar ley del redondeo
               "GlosaDR":"LeyDelRedondeo",
               "TpoValor":"$"
            }
         ],
         "Detalle":[
            {
               "CdgItem":{
                  "VlrCodigo":"9937993755000",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"ALMOHADILLA TERAP.CHICA",
               "QtyItem":5,
               "PrcItem":2790
            },
            {
               "CdgItem":{
                  "VlrCodigo":"9980808776653",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"SAL INGLESA 30GR.",
               "QtyItem":3,
               "PrcItem":690
            },
            {
               "UnmdItem":"UND",
               "PrcItem":1248,
               "CdgItem":{
                  "VlrCodigo":"99176480372",
                  "TpoCodigo":"INT1"
               },
               "NmbItem":"COLGATE TOT12 GEL WH.97GR",
               "DescuentoMonto":250,
               "QtyItem":2
            }
         ]
      }
   ]
}
```

### JSON Factura Electrónica

A continuación se da a conocer un ejemplo de Factura electrónica.

```js
{
   "Documentos":[
      {
         "Encabezado":{
            "IdDoc":{
                // Al indicar MntBruto implica que los precios se encuetran con IVA
                // para ocupar valores netos no se debe agregar este campo
               "MntBruto":"1",
               "TipoDTE":"33",
               "FchVenc":"2021-03-31",
               "TermPagoGlosa":"30 dias"
            },
            "Emisor":{
               "CdgVendedor":"Juan"
            },
            "Receptor":{
               "RznSocRecep":"CONTACTO INFORMATICA",
               "CmnaRecep":"Nunoa",
               "DirRecep":"San Eugenio 1331",
               "GiroRecep":"INFORMATICA",
               "RUTRecep":"78961710-4",
               "CiudadRecep":"Santiago",
               "Contacto":"Francisco"
            }
         },
         "Referencia":[
            {
               "TpoDocRef":"52",
               "FchRef":"2021-03-17",
               "FolioRef":"5",
               "RazonRef":"Facturación guia"
            }
         ],
         "DscRcgGlobal":[
            {
               "TpoMov":"D",
               "ValorDR":3382,
                // Glosa obligatoria para especificar descuento global
               "GlosaDR":"Descuento",
               "TpoValor":"$"
            },
            {
               "TpoMov":"D",
               "ValorDR":6,
                // Glosa obligatoria para especificar ley del redondeo
               "GlosaDR":"LeyDelRedondeo",
               "TpoValor":"$"
            }
         ],
         "Detalle":[
            {
               "CdgItem":{
                  "VlrCodigo":"9937993755000",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"ALMOHADILLA TERAP.CHICA",
               "QtyItem":"10",
               "PrcItem":2790
            },
            {
               "UnmdItem":"UND",
               "PrcItem":890,
               "CdgItem":{
                  "VlrCodigo":"9980808883221",
                  "TpoCodigo":"INT1"
               },
               "NmbItem":"MAGNESIO CLORURO 30GR",
               "DescuentoMonto":312,
               "QtyItem":"7"
            }
         ]
      }
   ]
}
```

### JSON Nota de crédito - Anulación total - Factura

 A continuación se da a conocer un ejemplo de Nota de crédito por anulación total.

```js
{
   "Documentos":[
      {
         "Encabezado":{
            "IdDoc":{
                // Al indicar MntBruto implica que los precios se encuetran con IVA
                // para ocupar valores netos no se debe agregar este campo
                "MntBruto":"1",
                "TipoDTE":"61",
                "TermPagoGlosa":"30 dias"
            },
            "Emisor":{
                "CdgVendedor":"Juan"
            },
            "Receptor":{
                "RznSocRecep":"CONTACTO INFORMATICA",
                "CmnaRecep":"San Bernardo",
                "RUTRecep":"78961710-4",
                "GiroRecep":"INFORMATICA",
                "DirRecep":"LINGUE 516",
                "CiudadRecep":"Santiago",
                "Contacto":"Francisco"
            }
         },
         "Referencia":[
            {
                "TpoDocRef":"33", 
                "FolioRef":"2043",
                "FchRef":"2021-03-31",
                "RazonRef":"Devolución de productos",
                // Este campo indica el tipo de nota de crédito
                // 1: Anula Documento de referencia (Anulacion total los montos de la NC === Factura o Boleta)
                // 2: Corrige Texto Documento de referencia
                // 3: Corrige montos ( Anulacion Parcial, los montos de la NC !== Factura o Boleta )
                "CodRef":"3"
            }
         ],
         "Detalle":[
            {
                "UnmdItem":"UND",
                "PrcItem":890,
                "CdgItem":{
                   "VlrCodigo":"9980808883221",
                   "TpoCodigo":"INT1"
                },
                "NmbItem":"MAGNESIO CLORURO 30GR",
                "DescuentoMonto":312,
                "QtyItem":7
            }
         ]
      }
   ]
}
```

### JSON Guía de despacho

 A continuación se da a conocer un ejemplo de la guia de despacho.

```js
{
   "Documentos":[
      {
         "Encabezado":{
            "IdDoc":{
                // El campo IndTraslado indica que tipo de guia se envíara
                //2: Ventas por efectuar
                //3: Consignaciones
                //4: Entrega gratuita
                //5: Traslados internos
                //6: Otros traslados no venta
                //7: Guía de devolución
                //8: Traslado para exportación. (no venta)
                //9: Venta para exportación
               "IndTraslado":"6",
                // Al indicar MntBruto implica que los precios se encuetran con IVA
                // para ocupar valores netos no se debe agregar este campo
               "MntBruto":"1",
               "TipoDTE":"52"
            },
            "Receptor":{
               "RznSocRecep":"CONTACTO INFORMATICA",
               "CmnaRecep":"Nunoa",
               "DirRecep":"SAN EUGENIO 1331",
               "GiroRecep":"INFORMATICA",
               "RUTRecep":"78961710-4",
               "CiudadRecep":"Santiago",
               "Contacto":"Jaime"
            },
            "Transporte":{
               "Patente":"ABCD123",
               "CiudadDest":"Santiago",
               "Chofer":{
                  "NombreChofer":"JUAN",
                  "RUTChofer":"11111111-1"
               },
               "DirDest":"PATRONATO 324",
               "CmnaDest":"Recoleta"
            }
         },
         "Detalle":[
            {
               "CdgItem":{
                  "VlrCodigo":"9937993755000",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"ALMOHADILLA TERAP.CHICA",
               "QtyItem":1,
               "PrcItem":2790
            },
            {
               "CdgItem":{
                  "VlrCodigo":"9980808776653",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"SAL INGLESA 30GR.",
               "QtyItem":1,
               "PrcItem":690
            },
            {
               "CdgItem":{
                  "VlrCodigo":"99176480372",
                  "TpoCodigo":"INT1"
               },
               "UnmdItem":"UND",
               "NmbItem":"COLGATE TOT12 GEL WH.97GR",
               "QtyItem":1,
               "PrcItem":1248
            }
         ]
      }
   ]
}
```

## Respuestas a la solicitud POST

A continuación se da a conocer los tipos de respuestas posibles entregadas por el servidor, las cuales se retornan en formato JSON.

###Tabla de respuestas:

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


Para los casos de éxito se incluye el campo 'documentos', que contiene los datos del documento enviado y generado.

##### Significado de los campos en la sección documentos del JSON de respuesta:

* **xml**: URL temporal que contiene el xml del documento
* **pdf**: URL temporal que contiene la versión impresa del documento
* **folio**: Folio asignado al documento
* **tipo**: Tipo de documento
                    
##### Ejemplo respuesta en caso de éxito

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
