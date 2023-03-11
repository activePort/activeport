---
cover: >-
  https://images.unsplash.com/photo-1630769989573-d7f20f0193b0?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxyZXNwb25zZXxlbnwwfHx8fDE2Nzg1NDQ2Njc&ixlib=rb-4.0.3&q=80
coverY: 0
---

# Solicitudes y Respuestas HTTP

Las comunicaciones HTTP consisten principalmente en una solicitud HTTP y una respuesta HTTP.  El cliente realiza una solicitud HTTP (p. ej., cURL/navegador) y la procesa el servidor (p. ej., servidor web). Las solicitudes contienen todos los detalles que requerimos del servidor, incluido el recurso (p. ej., URL, ruta, parámetros), cualquier dato de solicitud, encabezados u opciones que especifiquemos, y muchas otras opciones que analizaremos a lo largo de este módulo.

Una vez que el servidor recibe la solicitud HTTP, la procesa y responde enviando la respuesta HTTP, que contiene el código de respuesta, como se explica en una sección posterior, y puede contener los datos del recurso si el solicitante tuvo acceso a él.



## <mark style="color:blue;">Request HTTP</mark>

Comencemos examinando el siguiente ejemplo de solicitud HTTP:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

La imagen de arriba muestra una solicitud HTTP GET a la URL:

• `http://inlanefreight.com/users/login.html`

La primera línea de cualquier solicitud HTTP contiene tres campos principales 'separados por espacios':

| **Campo** | **Ejemplo**         | **Descripción**                                                                                                                   |
| --------- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `Method`  | `GET`               | El método HTTP o verbo, que especifica el tipo de acción a realizar.                                                              |
| `Path`    | `/users/login.html` | La ruta al recurso al que se accede. Este campo también puede tener como sufijo una cadena de consulta (p. ej. `?username=user`). |
| `Version` | `HTTP/1.1`          | El tercer y último campo se utiliza para indicar la versión HTTP.                                                                 |

El siguiente conjunto de líneas contiene pares de valores de encabezado. HTTP, como Host, User-Agent, Cookie y muchos otros encabezados posibles. Estos encabezados se utilizan para especificar varios atributos de una solicitud. Los encabezados terminan con una nueva línea, que es necesaria para que el servidor valide la solicitud. Finalmente, una solicitud puede terminar con el cuerpo y los datos de la solicitud.

{% hint style="info" %}
HTTP versión 1.X envía solicitudes como texto sin cifrar y utiliza un carácter de nueva línea para separar diferentes campos y diferentes solicitudes. HTTP versión 2.X, por otro lado, envía solicitudes como datos binarios en forma de diccionario.
{% endhint %}



## <mark style="color:blue;">HTTP Response</mark>

Una vez que el servidor procesa nuestra solicitud, envía su respuesta. El siguiente es un ejemplo de respuesta HTTP:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

La primera línea de una respuesta HTTP contiene dos campos separados por espacios. El primero es el `HTTP version`(p. ej `HTTP/1.1`.), y el segundo denota el `HTTP response code`(p. ej `200 OK`.).

Los códigos de respuesta se utilizan para determinar el estado de la solicitud, como se explicará en una sección posterior. Después de la primera línea, la respuesta enumera sus encabezados, de forma similar a una solicitud HTTP. Tanto los encabezados de solicitud como los de respuesta se analizan en la siguiente sección.

Finalmente, la respuesta puede terminar con un cuerpo de respuesta, que está separado por una nueva línea después de los encabezados. El cuerpo de la respuesta generalmente se define como `HTML`código. Sin embargo, también puede responder con otros tipos de código, como `JSON`recursos de sitios web como imágenes, hojas de estilo o scripts, o incluso un documento como un documento PDF alojado en el servidor web.



## <mark style="color:blue;">cURL</mark>

En nuestros ejemplos anteriores con cURL, solo especificamos la URL y obtuvimos el cuerpo de la respuesta a cambio. Sin embargo, cURL también nos permite obtener una vista previa de la solicitud HTTP completa y la respuesta HTTP completa, lo que puede ser muy útil al realizar pruebas de penetración web o escribir exploits. Para ver la solicitud y la respuesta HTTP completas, simplemente podemos agregar el <mark style="color:green;">`-v`</mark> indicador detallado a nuestros comandos anteriores, y debería imprimir tanto la solicitud como la respuesta:

```bash
activePort@htb[/htb]$ curl inlanefreight.com -v

*   Trying SERVER_IP:80...
* TCP_NODELAY set
* Connected to inlanefreight.com (SERVER_IP) port 80 (#0)
> GET / HTTP/1.1
> Host: inlanefreight.com
> User-Agent: curl/7.65.3
> Accept: */*
> Connection: close
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 401 Unauthorized
< Date: Tue, 21 Jul 2020 05:20:15 GMT
< Server: Apache/X.Y.ZZ (Ubuntu)
< WWW-Authenticate: Basic realm="Restricted Content"
< Content-Length: 464
< Content-Type: text/html; charset=iso-8859-1
< 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>

...SNIP...h
```

Como podemos ver, esta vez, obtenemos la solicitud y la respuesta HTTP completas. La solicitud simplemente se envía <mark style="color:green;">`GET / HTTP/1.1`</mark>  junto con los encabezados y <mark style="color:green;">`Host`</mark>. A cambio, la respuesta <mark style="color:green;">`HTTP`</mark> contenía el estado <mark style="color:green;">`401 Unauthorized`</mark>, lo que indica que no tenemos acceso sobre el recurso solicitado, como veremos en una próxima sección. Similar a la solicitud, la respuesta también contenía varios encabezados enviados por el servidor, incluidos <mark style="color:green;">`Date, Content-Length and Content-Type`</mark> . Finalmente, la respuesta contenía el cuerpo de la respuesta en HTML, que es el mismo que recibimos anteriormente cuando usamos cURL sin la bandera. <mark style="color:green;">`User-AgentAcceptHTTP/1.1 401 Unauthorized DateContent-LengthContent-Type-v.`</mark>



{% hint style="info" %}
La -vvv bandera muestra una salida aún más detallada. Intente usar esta marca para ver qué solicitud adicional y detalles de respuesta se muestran con ella.
{% endhint %}



## <mark style="color:blue;">Browser DevTools</mark>

La mayoría de los navegadores web modernos vienen con herramientas de desarrollador integradas ( `DevTools`), que están destinadas principalmente a que los desarrolladores prueben sus aplicaciones web. Sin embargo, como probadores de penetración web, estas herramientas pueden ser un activo vital en cualquier evaluación web que realicemos, ya que un navegador (y sus DevTools) se encuentran entre los activos que es más probable que tengamos en cada ejercicio de evaluación web. En este módulo, también analizaremos cómo utilizar algunas de las herramientas de desarrollo básicas del navegador para evaluar y monitorear diferentes tipos de solicitudes web.

Cada vez que visitamos un sitio web o accedemos a una aplicación web, nuestro navegador envía múltiples solicitudes web y maneja múltiples respuestas HTTP para mostrar la vista final que vemos en la ventana del navegador. Para abrir las herramientas de desarrollo del navegador en Chrome o Firefox, podemos hacer clic en \[ `CTRL+SHIFT+I`] o simplemente hacer clic en \[ `F12`]. Las herramientas de desarrollo contienen varias pestañas, cada una de las cuales tiene su propio uso. Nos centraremos principalmente en la `Network`pestaña de este módulo, ya que es responsable de las solicitudes web.

Si hacemos clic en la pestaña Red y actualizamos la página, deberíamos poder ver la lista de solicitudes enviadas por la página:

<figure><img src="../../.gitbook/assets/image (1) (1).jpeg" alt=""><figcaption></figcaption></figure>

Como podemos ver, las herramientas de desarrollo nos muestran de un vistazo el estado de la respuesta (es decir, el código de respuesta), el método de solicitud utilizado ( <mark style="color:green;">`GET`</mark>), el recurso solicitado (es decir, URL/dominio), junto con la ruta solicitada. Además, podemos usar <mark style="color:green;">`Filter URLs`</mark> para buscar una solicitud específica, en caso de que el sitio web cargue demasiados para revisar.
