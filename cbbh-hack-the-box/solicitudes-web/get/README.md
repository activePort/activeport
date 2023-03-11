---
cover: >-
  https://images.unsplash.com/photo-1527784011719-3349bf049cf4?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw2fHxnZXR8ZW58MHx8fHwxNjc4NTYyODE3&ixlib=rb-4.0.3&q=80
coverY: -123
---

# GET

Cada vez que visitamos cualquier URL, nuestros navegadores realizan una solicitud GET de forma predeterminada para obtener los recursos remotos alojados en esa URL. Una vez que el navegador recibe la página inicial que está solicitando; puede enviar otras solicitudes utilizando varios métodos HTTP. Esto se puede observar a través de la pestaña Red en las herramientas de desarrollo del navegador, como se vio en la sección anterior.

{% hint style="success" %}
Elija cualquier sitio web de su elección y controle la pestaña Red en las herramientas de desarrollo del navegador mientras lo visita para comprender el rendimiento de la página. Esta técnica se puede utilizar para comprender a fondo cómo una aplicación web interactúa con su backend, lo que puede ser un ejercicio esencial para cualquier evaluación de aplicaciones web o ejercicio de recompensas por errores.
{% endhint %}

## <mark style="color:orange;"></mark>

## <mark style="color:orange;">Autenticacion basica HTTP</mark>

Cuando visitamos el ejercicio que se encuentra al final de esta sección, nos solicita ingresar un nombre de usuario y una contraseña. A diferencia de los formularios de inicio de sesión habituales, que utilizan parámetros HTTP para validar las credenciales del usuario (p. ej., solicitud POST), este tipo de autenticación utiliza un correo electrónico <mark style="color:orange;">`basic HTTP authentication`</mark>, que el servidor web maneja directamente para proteger una página/directorio específico, sin interactuar directamente con la aplicación web.

Para acceder a la página, debemos ingresar un par de credenciales válidas, que son <mark style="color:orange;">`admin`</mark><mark style="color:orange;">:</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`admin`</mark>en este caso:

<figure><img src="../../../.gitbook/assets/image (1) (2).jpeg" alt=""><figcaption></figcaption></figure>

Una vez que ingresamos las credenciales, obtendríamos acceso a la página:

<figure><img src="../../../.gitbook/assets/image (6).jpeg" alt=""><figcaption></figcaption></figure>

Intentemos acceder a la página con cURL, y agregaremos <mark style="color:orange;">`-i`</mark> para ver los encabezados de respuesta:

```bash
activePort@htb[/htb]$ curl -i http://<SERVER_IP>:<PORT>/
HTTP/1.1 401 Authorization Required
Date: Mon, 21 Feb 2022 13:11:46 GMT
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-cache, must-revalidate, max-age=0
WWW-Authenticate: Basic realm="Access denied"
Content-Length: 13
Content-Type: text/html; charset=UTF-8

Access denied
```

Como podemos ver, entramos <mark style="color:orange;">`Access denied`</mark> en el cuerpo de la respuesta y también <mark style="color:orange;">`Basic realm="Access denied"`</mark> en el <mark style="color:orange;">`WWW-Authenticate`</mark> encabezado, lo que de hecho confirma que esta página sí usó <mark style="color:orange;">`basic HTTP auth`</mark>, como se discutió en la sección Encabezados. Para proporcionar las credenciales a través de cURL, podemos usar la <mark style="color:orange;">`-u`</mark> bandera, de la siguiente manera:

```bash
activePort@htb[/htb]$ curl -u admin:admin http://<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```

Esta vez obtenemos la página en la respuesta. Hay otro método en el que podemos proporcionar las <mark style="color:orange;">basic HTTP auth</mark> credenciales, que es directamente a través de la URL como (<mark style="color:orange;">username:password@URL</mark>), como comentamos en la primera sección. Si intentamos lo mismo con cURL o nuestro navegador, también obtendremos acceso a la página:

```bash
activePort@htb[/htb]$ curl http://admin:admin@<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```

También podemos intentar visitar la misma URL en un navegador, y también deberíamos autenticarnos.

{% hint style="info" %}
Intente ver los encabezados de respuesta agregando <mark style="color:orange;">`-i`</mark> a la solicitud anterior y vea cómo una respuesta autenticada difiere de una no autenticada.
{% endhint %}



## <mark style="color:orange;">Encabezado de autorizacion HTTP</mark>

Si agregamos el comando <mark style="color:orange;">`-v`</mark> a cualquiera de nuestros comandos cURL anteriores:

```bash
activePort@htb[/htb]$ curl -v http://admin:admin@<SERVER_IP>:<PORT>/

*   Trying <SERVER_IP>:<PORT>...
* Connected to <SERVER_IP> (<SERVER_IP>) port PORT (#0)
* Server auth using Basic with user 'admin'
> GET / HTTP/1.1
> Host: <SERVER_IP>
> Authorization: Basic YWRtaW46YWRtaW4=
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Mon, 21 Feb 2022 13:19:57 GMT
< Server: Apache/2.4.41 (Ubuntu)
< Cache-Control: no-store, no-cache, must-revalidate
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Pragma: no-cache
< Vary: Accept-Encoding
< Content-Length: 1453
< Content-Type: text/html; charset=UTF-8
< 

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```

Mientras usamos `basic HTTP auth`, vemos que nuestra solicitud HTTP establece el `Authorization`encabezado en `Basic YWRtaW46YWRtaW4=`, que es el valor codificado en base64 de `admin:admin`. Si estuviéramos usando un método moderno de autenticación (por ejemplo `JWT`, ), `Authorization`sería de tipo `Bearer`y contendría un token cifrado más largo.

Intentemos configurar manualmente el `Authorization`, sin proporcionar las credenciales, para ver si nos permite acceder a la página. Podemos configurar el encabezado con la `-H`bandera y usaremos el mismo valor de la solicitud HTTP anterior. Podemos agregar la `-H`bandera varias veces para especificar varios encabezados:

```bash
activePort@htb[/htb]$ curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/

<!DOCTYPE html
<html lang="en">

<head>
...SNIP...
```

Como vemos, esto también nos dio acceso a la página. Estos son algunos métodos que podemos usar para autenticarnos en la página. La mayoría de las aplicaciones web modernas utilizan formularios de inicio de sesión creados con el lenguaje de secuencias de comandos de back-end (por ejemplo, PHP), que utilizan solicitudes HTTP POST para autenticar a los usuarios y luego devuelven una cookie para mantener su autenticación.



## <mark style="color:orange;">Parametros GET</mark>

Una vez que estamos autenticados, tenemos acceso a la funcion `City Search`, en la que podemos ingresar un término de búsqueda y obtener una lista de ciudades coincidentes:

<figure><img src="../../../.gitbook/assets/image (6).jpeg" alt=""><figcaption></figcaption></figure>

A medida que la página devuelve nuestros resultados, puede ponerse en contacto con un recurso remoto para obtener la información y luego mostrarlos en la página. Para verificar esto, podemos abrir las herramientas de desarrollo del navegador e ir a la pestaña Red, o usar el acceso directo \[ CTRL+SHIFT+E] para llegar a la misma pestaña. Antes de ingresar nuestro término de búsqueda y ver las solicitudes, es posible que debamos hacer clic en el trashícono en la parte superior izquierda, para asegurarnos de eliminar las solicitudes anteriores y solo monitorear las solicitudes más nuevas.

<figure><img src="../../../.gitbook/assets/image (1).jpg" alt=""><figcaption></figcaption></figure>

Después de eso, podemos ingresar cualquier término de búsqueda y presionar enter, e inmediatamente notaremos que se envía una nueva solicitud al backend:

<figure><img src="../../../.gitbook/assets/image (5).jpeg" alt=""><figcaption></figcaption></figure>

Cuando hacemos clic en la solicitud, se envía `search.php`con el parámetro GET `search=le`utilizado en la URL. Esto nos ayuda a comprender que la función de búsqueda solicita otra página para los resultados.

Ahora, podemos enviar la misma solicitud directamente a `search.php`para obtener los resultados de búsqueda completos, aunque probablemente los devolverá en un formato específico (por ejemplo, JSON) sin tener el diseño HTML que se muestra en la captura de pantalla anterior.

Para enviar una solicitud GET con cURL, podemos usar exactamente la misma URL que se ve en las capturas de pantalla anteriores, ya que las solicitudes GET colocan sus parámetros en la URL. Sin embargo, las herramientas de desarrollo del navegador brindan un método más conveniente para obtener el comando cURL. Podemos hacer clic derecho sobre la solicitud y seleccionar `Copy>Copy as cURL`. Luego, podemos pegar el comando copiado en nuestra terminal y ejecutarlo, y deberíamos obtener exactamente la misma respuesta:

```bash
activePort@htb[/htb]$ curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='

Leeds (UK)
Leicester (UK)
```

{% hint style="info" %}
El comando copiado contendrá todos los encabezados utilizados en la solicitud HTTP. Sin embargo, podemos eliminar la mayoría de ellos y mantener solo los encabezados de autenticación necesarios, como el <mark style="color:orange;">`Authorization`</mark>encabezamiento.
{% endhint %}

También podemos repetir la solicitud exacta dentro de las herramientas de desarrollo del navegador, seleccionando <mark style="color:orange;">`Copy > Copy as Fetch`</mark>. Esto copiará la misma solicitud HTTP utilizando la biblioteca JavaScript Fetch. Luego, podemos ir a la pestaña de la consola de JavaScript haciendo clic en <mark style="color:orange;">`[ CTRL+SHIFT+K]`</mark>, pegue nuestro comando Fetch y presione enter para enviar la solicitud:

<figure><img src="../../../.gitbook/assets/image (3).jpeg" alt=""><figcaption></figcaption></figure>

Como vemos, el navegador envió nuestra solicitud y podemos ver la respuesta devuelta después. Podemos hacer clic en la respuesta para ver sus detalles, ampliar varios detalles y leerlos.
