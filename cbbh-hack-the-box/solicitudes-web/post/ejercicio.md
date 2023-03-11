---
cover: >-
  https://images.unsplash.com/photo-1517836357463-d25dfeac3438?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw4fHxleGVyY2lzZXxlbnwwfHx8fDE2Nzg1NDQ2MDA&ixlib=rb-4.0.3&q=80
coverY: 0
---

# Ejercicio

Propuesta : _Obtenga una cookie de sesión a través de un inicio de sesión válido y luego use la cookie con cURL para buscar la bandera a través de una solicitud JSON POST a  '/ search.php'._

__

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.03.30.png" alt=""><figcaption><p>Ip objetivo en este ejercicio</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.04.09.png" alt=""><figcaption><p>Credenciales</p></figcaption></figure>

Pongo el objetivo dado en la barra de navegacion y utilizo las credenciales dadas para poder autenticar.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.05.00.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.06.34.png" alt=""><figcaption></figcaption></figure>

Una vez dentro de la app me dirigo a la pestana Storage y copio las cookies de sesion.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.08.36.png" alt=""><figcaption></figcaption></figure>

El objetivo de obtener las cookies, es para no autenticarme cada vez que hago una peticion a la web. Como seria en este caso. Utilizando el comando <mark style="color:orange;">`-d`</mark> agrego los datos.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.13.34.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.14.04.png" alt=""><figcaption></figcaption></figure>

En esta ocasion, hago la peticion con la cookie y me devuelve exactamente lo mismo.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.15.59.png" alt=""><figcaption></figcaption></figure>

Ahora con la cookie en mano, realizamos un cURL a la web usando los comandos:

* <mark style="color:orange;">`-X POST`</mark> : Para enviar una solicitud POST.
* <mark style="color:orange;">`-d`</mark> : Para agregar los datos que se van a enviar.
* <mark style="color:orange;">`-b`</mark> : Agregar la cookie.
* <mark style="color:orange;">`-H`</mark> : Configuramos el Header.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 16.25.21.png" alt=""><figcaption></figcaption></figure>

Y obtengo la bnadera.
