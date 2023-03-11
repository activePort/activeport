# HTTPS

## <mark style="color:blue;">Hypertext Transfer Protocol Secure (HTTPS)</mark>

En la sección anterior, discutimos cómo se envían y procesan las solicitudes HTTP. Sin embargo, uno de los inconvenientes importantes de HTTP es que todos los datos se transfieren en texto claro. Esto significa que cualquier persona entre el origen y el destino puede realizar un ataque Man-in-the-middle (MiTM) para ver los datos transferidos.

Para contrarrestar este problema, se creó el [protocolo HTTPS (HTTP Secure)](https://tools.ietf.org/html/rfc2660) , en el que todas las comunicaciones se transfieren en un formato encriptado, por lo que incluso si un tercero intercepta la solicitud, no podrá extraer los datos. Por esta razón, HTTPS se ha convertido en el esquema principal para sitios web en Internet, y HTTP se está eliminando gradualmente, y pronto la mayoría de los navegadores web no permitirán visitar sitios web HTTP.



## <mark style="color:blue;">Descripcion general de HTTPS</mark>

Si examinamos una solicitud HTTP, podemos ver el efecto de no aplicar comunicaciones seguras entre un navegador web y una aplicación web. Por ejemplo, el siguiente es el contenido de una solicitud de inicio de sesión HTTP:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Podemos ver que las credenciales de inicio de sesión se pueden ver en texto claro. Esto facilitaría que alguien en la misma red (como una red inalámbrica pública) capture la solicitud y reutilice las credenciales con fines maliciosos.

Por el contrario, cuando alguien intercepta y analiza el tráfico de una solicitud HTTPS, vería algo como lo siguiente:

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Como podemos ver, los datos se transfieren como un único flujo cifrado, lo que hace que sea muy difícil para cualquiera capturar información como credenciales o cualquier otro dato confidencial.

Los sitios web que aplican HTTPS se pueden identificar a través https:// de su URL (por ejemplo, https://www.google.com), así como del icono de candado en la barra de direcciones del navegador web, a la izquierda de la URL:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Entonces, si visitamos un sitio web que utiliza HTTPS, como Google, todo el tráfico estaría encriptado.

{% hint style="warning" %}
Aunque los datos transferidos a través del protocolo HTTPS pueden estar encriptados, la solicitud aún puede revelar la URL visitada si se contactó con un servidor DNS de texto sin cifrar. Por esta razón, se recomienda utilizar servidores DNS encriptados (p. ej., 8.8.8.8 o 1.2.3.4) o utilizar un servicio VPN para garantizar que todo el tráfico esté encriptado correctamente.
{% endhint %}



## <mark style="color:blue;">Flujo HTTPS</mark>

Veamos cómo funciona HTTPS a un alto nivel:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Si escribimos http:// en lugar de https:// al visitar un sitio web que aplica HTTPS, el navegador intenta resolver el dominio y redirige al usuario al servidor web que aloja el sitio web de destino. Primero se envía una solicitud al puerto 80, que es el protocolo HTTP sin cifrar. El servidor detecta esto y redirige al cliente a un puerto HTTPS seguro 443. Esto se hace a través del `301 Moved Permanently` código de respuesta, que discutiremos en una próxima sección.

A continuación, el cliente (navegador web) envía un paquete de "hola del cliente", brindando información sobre sí mismo. Después de esto, el servidor responde con "servidor hola", seguido de un intercambio de [claves](https://en.wikipedia.org/wiki/Key\_exchange) para intercambiar certificados SSL. El cliente verifica la clave/certificado y envía uno propio. Después de esto, se inicia un [protocolo](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake) de enlace cifrado para confirmar si el cifrado y la transferencia funcionan correctamente.

Una vez que el protocolo de enlace se completa con éxito, continúa la comunicación HTTP normal, que se cifra después de eso. Esta es una descripción general de muy alto nivel del intercambio de claves, que está más allá del alcance de este módulo.

{% hint style="info" %}
Dependiendo de las circunstancias, un atacante puede realizar un ataque de degradación de HTTP, que degrada la comunicación de HTTPS a HTTP, haciendo que los datos se transfieran en texto claro. Esto se hace configurando un proxy Man-In-The-Middle (MITM) para transferir todo el tráfico a través del host del atacante sin el conocimiento del usuario. Sin embargo, la mayoría de los navegadores, servidores y aplicaciones web modernos protegen contra este ataque.
{% endhint %}



## <mark style="color:blue;">cURL para HTTPS</mark>

cURL debe manejar automáticamente todos los estándares de comunicación HTTPS y realizar un protocolo de enlace seguro y luego cifrar y descifrar los datos automáticamente. Sin embargo, si alguna vez nos ponemos en contacto con un sitio web con un certificado SSL no válido o desactualizado, cURL de forma predeterminada no procederá con la comunicación para protegerse contra los ataques MITM mencionados anteriormente:

```bash
activePort@htb[/htb]$ curl https://inlanefreight.com

curl: (60) SSL certificate problem: Invalid certificate chain
More details here: https://curl.haxx.se/docs/sslcerts.html
...SNIP...
```

Los navegadores web modernos harían lo mismo, advirtiendo al usuario que no visite un sitio web con un certificado SSL no válido.

Es posible que enfrentemos un problema de este tipo al probar una aplicación web local o con una aplicación web alojada con fines prácticos, ya que es posible que dichas aplicaciones web aún no hayan implementado un certificado SSL válido. Para omitir la verificación del certificado con cURL, podemos usar la <mark style="color:green;">`-k`</mark> bandera:

```bash
activePort@htb[/htb]$ curl -k https://inlanefreight.com

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
```

Como podemos ver, la solicitud pasó esta vez y recibimos los datos de respuesta.
