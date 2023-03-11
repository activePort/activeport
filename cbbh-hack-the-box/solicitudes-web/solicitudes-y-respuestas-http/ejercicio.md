---
cover: >-
  https://images.unsplash.com/photo-1517836357463-d25dfeac3438?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw4fHxleGVyY2lzZXxlbnwwfHx8fDE2Nzg1NDQ2MDA&ixlib=rb-4.0.3&q=80
coverY: 0
---

# Ejercicio

Propuesta : 1 ) - _¿Cuál es el método HTTP utilizado al interceptar la solicitud?_

_2) - Envíe una solicitud GET al servidor anterior y lea los encabezados de respuesta para encontrar la versión de Apache que se ejecuta en el servidor, luego envíela como respuesta._

__

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 11.16.21.png" alt=""><figcaption><p>Ip objetivo para este ejercicio</p></figcaption></figure>

Pego la ip en la URL e inspeciono la pagina para obtener el metodo que se realizo.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 11.17.38.png" alt=""><figcaption></figcaption></figure>

Luego de eso voy a la terminal y con cURL y el comando -v puedo leer los encabezados de la respuesta y ver la version del servidor Apache que estan utilizando.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 11.20.10.png" alt=""><figcaption></figcaption></figure>
