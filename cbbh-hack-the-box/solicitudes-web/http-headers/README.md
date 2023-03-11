---
cover: >-
  https://images.unsplash.com/photo-1568725386896-a38b401cbd8c?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxoZWFkZXJzfGVufDB8fHx8MTY3ODU0NDcxNA&ixlib=rb-4.0.3&q=80
coverY: 0
---

# HTTP Headers

Hemos visto ejemplos de solicitudes <mark style="color:green;">`HTTP`</mark> y encabezados de respuesta en la sección anterior. Dichos encabezados <mark style="color:green;">`HTTP`</mark> pasan información entre el cliente y el servidor. Algunos encabezados solo se usan con solicitudes o respuestas, mientras que otros encabezados generales son comunes a ambos.

Los encabezados pueden tener uno o varios valores, agregados después del nombre del encabezado y separados por dos puntos. Podemos dividir los encabezados en las siguientes categorías:

1. <mark style="color:green;">`General Headers`</mark>
2. <mark style="color:green;">`Entity Headers`</mark>
3. <mark style="color:green;">`Request Headers`</mark>
4. <mark style="color:green;">`Response Headers`</mark>
5. <mark style="color:green;">`Security Headers`</mark>

Analicemos cada una de estas categorías.



## <mark style="color:blue;">General Headers (Encabezados Generales)</mark>

[Los encabezados generales](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html) se utilizan tanto en las solicitudes como en las respuestas HTTP. Son contextuales y describen el mensaje más que su contenido.



| **Encabezamiento** | **Ejemplo**                           | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------ | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Date`             | `Date: Wed, 16 Feb 2022 10:38:44 GMT` | Contiene la fecha y la hora en que se originó el mensaje. Es preferible convertir la hora a la [UTC estándar. ](https://en.wikipedia.org/wiki/Coordinated\_Universal\_Time)zona horaria                                                                                                                                                                                                           |
| `Connection`       | `Connection: close`                   | Dicta si la conexión de red actual debe permanecer activa después de que finalice la solicitud. Dos valores de uso común para este encabezado son `close`y `keep-alive`. los `close`valor del cliente o del servidor significa que les gustaría terminar la conexión, mientras que el `keep-alive`El encabezado indica que la conexión debe permanecer abierta para recibir más datos y entradas. |

<mark style="color:blue;"></mark>

## <mark style="color:blue;">Entity Headers (Encabezados de entidad)</mark>

Al igual que los encabezados generales, los encabezados de [entidad](https://www.w3.org/Protocols/rfc2616/rfc2616-sec7.html) pueden ser iguales tanto para las solicitudes y las respuestas. Estos encabezados se utilizan para escribir el contenido(entidad) transferidos por un mensaje. Por lo general, se encuentran en respuestas y solicitudes POST o PUT.

| **Encabezamiento** | **Ejemplo**                   | **Descripción**                                                                                                                                                                                                                                                                                    |
| ------------------ | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Content-Type`     | `Content-Type: text/html`     | Se utiliza para describir el tipo de recurso que se transfiere. Los navegadores agregan automáticamente el valor en el lado del cliente y lo devuelven en la respuesta del servidor. los `charset`El campo indica el estándar de codificación, como [UTF-8 ](https://en.wikipedia.org/wiki/UTF-8). |
| `Media-Type`       | `Media-Type: application/pdf` | los `media-type`es parecido a `Content-Type`y describe los datos que se transfieren. Este encabezado puede desempeñar un papel crucial para que el servidor interprete nuestra entrada. los `charset`El campo también se puede utilizar con este encabezado.                                       |
| `Boundary`         | `boundary="b4e4fbd93540"`     | Actúa como creador para separar el contenido cuando hay más de uno en el mismo mensaje. Por ejemplo, dentro de los datos de un formulario, este límite se usa como `--b4e4fbd93540`para separar las diferentes partes del formulario.                                                              |
| `Content-Length`   | `Content-Length: 385`         | Contiene el tamaño de la entidad que se está pasando. Este encabezado es necesario ya que el servidor lo usa para leer datos del cuerpo del mensaje y el navegador y herramientas como cURL lo generan automáticamente.                                                                            |
| `Content-Encoding` | `Content-Encoding: gzip`      | Los datos pueden sufrir múltiples transformaciones antes de pasar. Por ejemplo, se pueden comprimir grandes cantidades de datos para reducir el tamaño del mensaje. El tipo de codificación que se utiliza debe especificarse utilizando el `Content-Encoding`encabezamiento.                      |



## <mark style="color:blue;">Request Headers (Encabezados de solicitud)</mark>

El cliente envía [encabezados de solicitud](https://tools.ietf.org/html/rfc2616) en una transacción HTTP. Estos encabezados son utilizados en una solicitud HTTP y no están relacionados con el contenido del mensaje. Los siguientes encabezados se ven comúnmente en las solicitudes HTTP.

| **Encabezamiento** | **Ejemplo**                              | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Host`             | `Host: www.inlanefreight.com`            | Se utiliza para especificar el host que se consulta por el recurso. Puede ser un nombre de dominio o una dirección IP. Los servidores HTTP se pueden configurar para alojar diferentes sitios web, que se revelan en función del nombre de host. Esto hace que el encabezado del host sea un objetivo de enumeración importante, ya que puede indicar la existencia de otros hosts en el servidor de destino.                                                                                                                   |
| `User-Agent`       | `User-Agent: curl/7.77.0`                | Los `User-Agent` el encabezado se utiliza para describir el cliente que solicita recursos. Este encabezado puede revelar mucho sobre el cliente, como el navegador, su versión y el sistema operativo.                                                                                                                                                                                                                                                                                                                          |
| `Referer`          | `Referer: http://www.inlanefreight.com/` | Indica de dónde proviene la solicitud actual. Por ejemplo, hacer clic en un enlace de los resultados de búsqueda de Google haría que `https://google.com`el referente Confiar en este encabezado puede ser peligroso, ya que se puede manipular fácilmente y tener consecuencias no deseadas.                                                                                                                                                                                                                                   |
| `Accept`           | `Accept: */*`                            | <p>El <code>Accept</code> encabezado describe qué tipos de medios puede entender el cliente. Puede contener múltiples tipos de medios separados por comas. El <code>*/*</code> </p><p>valor significa que se aceptan todos los tipos de medios.</p>                                                                                                                                                                                                                                                                             |
| `Cookie`           | `Cookie: PHPSESSID=b4e4fbd93540`         | Contiene pares de valor de cookie en el formato `name=value`. Una [cookie ](https://en.wikipedia.org/wiki/HTTP\_cookie)es una pieza de datos almacenada en el lado del cliente y en el servidor, que actúa como un identificador. Estos se pasan al servidor por solicitud, manteniendo así el acceso del cliente. Las cookies también pueden servir para otros fines, como guardar las preferencias del usuario o el seguimiento de la sesión. Puede haber varias cookies en un solo encabezado separadas por un punto y coma. |
| `Authorization`    | `Authorization: BASIC cGFzc3dvcmQK`      | Otro método para que el servidor identifique a los clientes. Después de una autenticación exitosa, el servidor devuelve un token único para el cliente. A diferencia de las cookies, los tokens se almacenan solo en el lado del cliente y el servidor los recupera por solicitud. Hay varios tipos de tipos de autenticación según el servidor web y el tipo de aplicación utilizados.                                                                                                                                         |

Puede encontrar una lista completa de encabezados de solicitud y su uso [aquí](https://tools.ietf.org/html/rfc7231#section-5) .



## <mark style="color:blue;">Encabezados de Respuesta (Response Headers)</mark>

[Los encabezados de respuesta](https://tools.ietf.org/html/rfc7231#section-6) pueden ser utilizados en una respuesta HTTP y no están relacionados con el contenido. Ciertos encabezados de respuesta como Age, Locationy Serverse usan para proporcionar más contexto sobre la respuesta. Los siguientes encabezados se ven comúnmente en las respuestas HTTP.

| **Encabezamiento** | **Ejemplo**                                 | **Descripción**                                                                                                                                                                                                                   |
| ------------------ | ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Server`           | `Server: Apache/2.2.14 (Win32)`             | Contiene información sobre el servidor HTTP que procesó la solicitud. Se puede usar para obtener información sobre el servidor, como su versión, y enumerarlo más.                                                                |
| `Set-Cookie`       | `Set-Cookie: PHPSESSID=b4e4fbd93540`        | Contiene las cookies necesarias para la identificación del cliente. Los navegadores analizan las cookies y las almacenan para futuras solicitudes. Este encabezado sigue el mismo formato que el `Cookie`encabezado de solicitud. |
| `WWW-Authenticate` | `WWW-Authenticate: BASIC realm="localhost"` | Notifica al cliente sobre el tipo de autenticación requerida para acceder al recurso solicitado.                                                                                                                                  |



## <mark style="color:blue;">Encabezados de seguridad (Header Security)</mark>

Por último, tenemos [los encabezados de seguridad](https://owasp.org/www-project-secure-headers/) . Con el aumento en la variedad de navegadores y ataques basados en web, era necesario definir ciertos encabezados que mejoraran la seguridad. Los encabezados de seguridad HTTP son una clase de cabeceras de respuesta utilizadas para especificar determinadas normas y políticas que deben de ser seguidos por el navegador al acceder al sitio web.

| **Encabezamiento**          | **Ejemplo**                                   | **Descripción**                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Content-Security-Policy`   | `Content-Security-Policy: script-src 'self'`  | Dicta la política del sitio web con respecto a los recursos inyectados externamente. Esto podría ser código JavaScript, así como recursos de secuencias de comandos. Este encabezado le indica al navegador que acepte recursos solo de ciertos dominios confiables, lo que evita ataques como [Cross-site scripting (XSS) ](https://en.wikipedia.org/wiki/Cross-site\_scripting). |
| `Strict-Transport-Security` | `Strict-Transport-Security: max-age=31536000` | Evita que el navegador acceda al sitio web a través del protocolo HTTP de texto sin formato y obliga a que todas las comunicaciones se realicen a través del protocolo seguro HTTPS. Esto evita que los atacantes detecten el tráfico web y accedan a información protegida, como contraseñas u otros datos confidenciales.                                                        |
| `Referrer-Policy`           | `Referrer-Policy: origin`                     | Dicta si el navegador debe incluir el valor especificado a través del `Referer`cabecera o no. Puede ayudar a evitar la divulgación de URL e información confidencial mientras navega por el sitio web.                                                                                                                                                                             |

{% hint style="info" %}
Esta sección solo menciona un pequeño subconjunto de encabezados HTTP comúnmente vistos. Hay muchos otros encabezados contextuales que se pueden usar en las comunicaciones HTTP. También es posible que las aplicaciones definan encabezados personalizados según sus requisitos. Puede encontrar una lista completa de encabezados HTTP estándar [aquí ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).
{% endhint %}



## <mark style="color:blue;">cURL</mark>

En la sección anterior, vimos cómo usar el `-v`flag con cURL nos muestra los detalles completos de la solicitud y respuesta HTTP. Si solo estuviéramos interesados ​​en ver los encabezados de respuesta, entonces podemos usar el `-I`bandera para enviar un `HEAD`solicitar y solo mostrar los encabezados de respuesta. Además, podemos utilizar el `-i`bandera para mostrar tanto los encabezados como el cuerpo de la respuesta (por ejemplo, código HTML). La diferencia entre los dos es que `-I`envía un `HEAD`solicitud (como se verá en la siguiente sección), mientras que `-i`envía cualquier solicitud que especifiquemos e imprime los encabezados también.

El siguiente comando muestra un ejemplo de salida del uso del comando `-I`:

```bash
activePort@htb[/htb]$ curl -I https://www.inlanefreight.com

Host: www.inlanefreight.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko)
Cookie: cookie1=298zf09hf012fh2; cookie2=u32t4o3tb3gg4
Accept: text/plain
Referer: https://www.inlanefreight.com/
Authorization: BASIC cGFzc3dvcmQK

Date: Sun, 06 Aug 2020 08:49:37 GMT
Connection: keep-alive
Content-Length: 26012
Content-Type: text/html; charset=ISO-8859-4
Content-Encoding: gzip
Server: Apache/2.2.14 (Win32)
Set-Cookie: name1=value1,name2=value2; Expires=Wed, 09 Jun 2021 10:18:14 GMT
WWW-Authenticate: BASIC realm="localhost"
Content-Security-Policy: script-src 'self'
Strict-Transport-Security: max-age=31536000
Referrer-Policy: origin
```



{% hint style="success" %}
Intente revisar todos los encabezados anteriores y vea si puede recordar el uso de cada uno de ellos.
{% endhint %}



Además de ver encabezados, <mark style="color:green;">`cURL`</mark> también nos permite establecer encabezados de solicitud con el comando <mark style="color:green;">`-H`</mark> , como veremos en una sección posterior. Algunos encabezados, como los encabezados <mark style="color:green;">`User-Agent`</mark> o <mark style="color:green;">`Cookie`</mark>, tienen sus propias banderas. Por ejemplo, podemos usar <mark style="color:green;">`-A`</mark> para configurar nuestro <mark style="color:green;">`User-Agent`</mark>, de la siguiente manera:

```bash
activePort@htb[/htb]$ curl https://www.inlanefreight.com -A 'Mozilla/5.0'

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
```

{% hint style="info" %}
Trate de usar el <mark style="color:green;">`-I`</mark>o el <mark style="color:green;">`-v`</mark>flags con el ejemplo anterior, para asegurarnos de que cambiamos nuestro <mark style="color:green;">`User-Agent`</mark> con el <mark style="color:green;">`-A`</mark>bandera.
{% endhint %}



## <mark style="color:blue;">Herramientas de desarollo del navegador</mark>

Finalmente, veamos cómo podemos obtener una vista previa de los encabezados HTTP usando las herramientas de desarrollo del navegador. Al igual que hicimos en el apartado anterior, podemos ir a la pestaña Network para ver las diferentes solicitudes que realiza la página. Podemos hacer clic en cualquiera de las solicitudes para ver su detalle:

<figure><img src="../../../.gitbook/assets/image (3).jpeg" alt=""><figcaption></figcaption></figure>

En la primera <mark style="color:green;">`Headers`</mark> pestaña, vemos tanto la solicitud HTTP como los encabezados de respuesta HTTP. Las herramientas de desarrollo organizan automáticamente los encabezados en secciones, pero podemos hacer clic en el <mark style="color:green;">`Raw`</mark> botón para ver sus detalles en su formato sin procesar. Además, podemos consultar la <mark style="color:green;">`Cookies`</mark> pestaña para ver las cookies utilizadas por la solicitud, como se explica en una próxima sección.
