En este documento mostraré como consumir la API del Banco Central de la Republica Argentina con XOJO.
Esto puede ser muy útil para obtener la cotización Oficial del dolar del día, y varias consultas mas que se mencionaran.

1- Para poder consumir esta API es requisito autenticarse usando un token cada vez que se realice una llamada. Dicho token nos lo provee el BCRA de manera gratuita, solo hay que registrarse con una cuenta de mail.

link de registro: http://estadisticasbcra.com/api/registracion

2 - Una vez registrados, la página del BCRA nos dará un token válido por un año desde la fecha de registro. Pasado el año será necesario registrar otro. El token está limitado a 100 consultas diarias.

3- Utilizando XOJO podemos crear una aplicacion donde:

  a) Agregamos un componente HTTPSocket
  
  b) Un botón desde donde invocaremos la API a consumir
  
  c) A los efectos de visualizar el resultado de la respuesta de la API (o sea el JSON recibido como respuesta) agregar un TextArea que se podrá llamar por ejemplo txtRespuestaAPI
  
  d) En el evento Action del botón colocamos el siguiente código (por ejemplo para obtener las cotizaciones del dolar oficial):
  
        Dim url As Text = "https://api.estadisticasbcra.com/usd_of"
        
        OpenALPRSocket.RequestHeader("Authorization") = "BEARER token_recibido_del_BCRA"
        
        OpenALPRSocket.Send("GET", url)
        
Lo que estamos haciendo aqui es definir la URL de la API a consumir, luego poniendo en el Header los codigos de autorizacion necesarios para utilizar el servicio, y por ultimo invocando el servicio
  
  e) En el evento PageReceive del HTTPSocket colocamos el siguiente código
  
        Dim result As Text = Xojo.Core.TextEncoding.UTF8.ConvertDataToText(Content)
        
        txtRespuestaAPI.Text = result

De esta manera se visualizará en el objeto TextArea el JSON reciido como respuesta, que en este caso contendrá todas las cotizaciones del dolar oficial con la siguiente estructura:
        [
          {
            "d": fecha en formato MySQL,
            "v": valor del indicador para esa fecha.
          }
        ]

La documentación oficial de la API provista por el BCRA con todas las consultas posibles está en:
    http://estadisticasbcra.com/api/documentacion
    

