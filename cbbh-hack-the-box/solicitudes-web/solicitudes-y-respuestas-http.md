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
