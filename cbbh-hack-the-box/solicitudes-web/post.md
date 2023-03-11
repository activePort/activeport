# POST

En la sección anterior, vimos cómo las solicitudes `GET` pueden ser utilizadas por aplicaciones web para funcionalidades como búsqueda y acceso a páginas. Sin embargo, cada vez que las aplicaciones web necesitan transferir archivos o mover los parámetros de usuario de la URL, utilizan peticiones `POST`.

A diferencia de HTTP GET, que coloca los parámetros de usuario dentro de la URL, HTTP POSTcoloca los parámetros de usuario dentro del cuerpo de la solicitud HTTP. Esto tiene tres beneficios principales:

* `Lack of Logging`: Dado que las solicitudes POST pueden transferir archivos grandes (por ejemplo, carga de archivos), no sería eficiente que el servidor registrara todos los archivos cargados como parte de la URL solicitada, como sería el caso de un archivo cargado a través de una solicitud GET.
* `Less Encoding Requirements`: Las direcciones URL están diseñadas para compartirse, lo que significa que deben ajustarse a los caracteres que se pueden convertir en letras. La solicitud POST coloca datos en el cuerpo que pueden aceptar datos binarios. Los únicos caracteres que deben codificarse son los que se utilizan para separar parámetros.
* `More data can be sent`: La longitud máxima de URL varía entre navegadores (Chrome/Firefox/IE), servidores web (IIS, Apache, nginx), redes de entrega de contenido (Fastly, Cloudfront, Cloudflare) e incluso acortadores de URL ([bit.ly](http://bit.ly), [amzn.to](http://amzn.to)). En términos generales, la longitud de una URL debe mantenerse por debajo de los 2000 caracteres, por lo que no pueden manejar una gran cantidad de datos.

Entonces, veamos algunos ejemplos de cómo funcionan las solicitudes POST y cómo podemos utilizar herramientas como cURL o herramientas de desarrollo del navegador para leer y enviar solicitudes POST.



## <mark style="color:orange;">Formularios de inicio de sesion</mark>

El ejercicio al final de esta sección es similar al ejemplo que vimos en la sección GET. Sin embargo, una vez que visitamos la aplicación web, vemos que utiliza un formulario de inicio de sesión PHP en lugar de autenticación básica HTTP:

<figure><img src="../../.gitbook/assets/image (5).jpeg" alt=""><figcaption></figcaption></figure>

