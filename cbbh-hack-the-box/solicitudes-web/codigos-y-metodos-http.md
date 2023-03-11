---
cover: >-
  https://images.unsplash.com/photo-1580163661417-3606299aba72?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwzfHxodHRwfGVufDB8fHx8MTY3ODU0NDU3NA&ixlib=rb-4.0.3&q=80
coverY: 90
---

# Codigos y metodos HTTP

HTTP admite múltiples métodos para acceder a un recurso. En el protocolo HTTP, varios métodos de solicitud permiten que el navegador envíe información, formularios o archivos al servidor. Estos métodos se utilizan, entre otras cosas, para decirle al servidor cómo procesar la solicitud que enviamos y cómo responder.

Vimos diferentes métodos HTTP utilizados en las solicitudes HTTP que probamos en las secciones anteriores. Con cURL, si usamos <mark style="color:orange;">`-v`</mark> para obtener una vista previa de la solicitud completa, la primera línea contiene el método HTTP (p. ej <mark style="color:orange;">`GET / HTTP/1.1`</mark>.), mientras que con las herramientas de desarrollo del navegador, el método HTTP se muestra en la columna <mark style="color:orange;">`Method`</mark>. Además, los encabezados de respuesta también contienen el código de respuesta HTTP, que indica el estado del procesamiento de nuestra solicitud HTTP.



## <mark style="color:orange;">Metodos de solicitud</mark>

Los siguientes son algunos de los métodos comúnmente utilizados:

| **Método**                                | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <mark style="color:orange;">`GET`</mark>  | Solicita un recurso específico. Se pueden pasar datos adicionales al servidor a través de cadenas de consulta en la URL (p. ej. `?param=value`).                                                                                                                                                                                                                                                 |
| <mark style="color:orange;">`POST`</mark> | Envía datos al servidor. Puede manejar múltiples tipos de entrada, como texto, archivos PDF y otras formas de datos binarios. Estos datos se adjuntan en el cuerpo de la solicitud presente después de los encabezados. El método POST se usa comúnmente cuando se envía información (por ejemplo, formularios/inicios de sesión) o se cargan datos en un sitio web, como imágenes o documentos. |
| <mark style="color:orange;">`HEAD`</mark> | Solicita los encabezados que se devolverían si se hiciera una solicitud GET al servidor. No devuelve el cuerpo de la solicitud y, por lo general, se realiza para verificar la longitud de la respuesta antes de descargar los recursos.                                                                                                                                                         |
| <mark style="color:orange;">`PUT`</mark>  | Crea nuevos recursos en el servidor. Permitir este método sin los controles adecuados puede conducir a la carga de recursos maliciosos.                                                                                                                                                                                                                                                          |
| `DELETE`                                  | Elimina un recurso existente en el servidor web. Si no se protege adecuadamente, puede provocar una denegación de servicio (DoS) al eliminar archivos críticos en el servidor web.                                                                                                                                                                                                               |
| `OPTIONS`                                 | Devuelve información sobre el servidor, como los métodos aceptados por este.                                                                                                                                                                                                                                                                                                                     |
| `PATCH`                                   | Aplica modificaciones parciales al recurso en la ubicación especificada.                                                                                                                                                                                                                                                                                                                         |

La lista solo destaca algunos de los métodos HTTP más utilizados. La disponibilidad de un método en particular depende del servidor y de la configuración de la aplicación. Para obtener una lista completa de los métodos HTTP, puede visitar este [enlace](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

{% hint style="info" %}
La mayoría de las aplicaciones web modernas se basan principalmente en los métodos GET y POST. Sin embargo, cualquier aplicación web que utilice las API REST también se basa en PUT y DELETE, que se utilizan para actualizar y eliminar datos en el extremo de la API, respectivamente.
{% endhint %}



## <mark style="color:orange;">Codigos de respuesta</mark>

Los códigos de estado HTTP se utilizan para informar al cliente sobre el estado de su solicitud. Un servidor HTTP puede devolver cinco tipos de códigos de respuesta:



| **Escribe** | **Descripción**                                                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `1xx`       | Proporciona información y no afecta el procesamiento de la solicitud.                                                                    |
| `2xx`       | Devuelto cuando una solicitud tiene éxito.                                                                                               |
| `3xx`       | Devuelto cuando el servidor redirige al cliente.                                                                                         |
| `4xx`       | Significa solicitudes inapropiadas `from the client`. Por ejemplo, solicitar un recurso que no existe o solicitar un formato incorrecto. |
| `5xx`       | Devuelto cuando hay algún problema. `with the HTTP server`sí mismo.                                                                      |

Los siguientes son algunos de los ejemplos más comunes de cada uno de los tipos de métodos HTTP anteriores:

| **Código**                  | **Descripción**                                                                                                                                       |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `200 OK`                    | Devuelto en una solicitud exitosa, y el cuerpo de la respuesta generalmente contiene el recurso solicitado.                                           |
| `302 Found`                 | Redirige al cliente a otra URL. Por ejemplo, redirigir al usuario a su tablero después de un inicio de sesión exitoso.                                |
| `400 Bad Request`           | Devuelto al encontrar solicitudes con formato incorrecto, como solicitudes con terminadores de línea faltantes.                                       |
| `403 Forbidden`             | Significa que el cliente no tiene acceso adecuado al recurso. También se puede devolver cuando el servidor detecta una entrada maliciosa del usuario. |
| `404 Not Found`             | Devuelto cuando el cliente solicita un recurso que no existe en el servidor.                                                                          |
| `500 Internal Server Error` | Devuelto cuando el servidor no puede procesar la solicitud.                                                                                           |

Para obtener una lista completa de los códigos de respuesta HTTP estándar, puede visitar este [enlace](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) . Aparte de los códigos HTTP estándar, varios servidores y proveedores como [Cloudflare](https://support.cloudflare.com/hc/en-us/articles/115003014432-HTTP-Status-Codes) o [AWS](https://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/APIError.html) implementan sus propios códigos.
