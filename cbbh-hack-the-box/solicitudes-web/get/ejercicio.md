---
cover: >-
  https://images.unsplash.com/photo-1517836357463-d25dfeac3438?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw4fHxleGVyY2lzZXxlbnwwfHx8fDE2Nzg1NDQ2MDA&ixlib=rb-4.0.3&q=80
coverY: 0
---

# Ejercicio

Propuesta : _El ejercicio anterior parece estar roto, ya que arroja resultados incorrectos. Use las herramientas de desarrollo del navegador para ver cuál es la solicitud que envía cuando buscamos, y use cURL para buscar 'flag' y obtener la bandera._

__

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.26.18.png" alt=""><figcaption><p>Ip objetivo para este ejercicio.</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.29.16.png" alt=""><figcaption><p>Credenciales dadas.</p></figcaption></figure>



Coloco la ip en la barra de navegación y se me pide iniciar sesion con unas credenciales.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.30.48.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.31.27.png" alt=""><figcaption></figcaption></figure>

Dentro de la web, ingreso en el formulario de busqueda la palabra <mark style="color:orange;">`flag`</mark> <mark style="color:orange;"></mark> <mark style="color:orange;"></mark><mark style="color:orange;"></mark> . Inspecciono la pagina y voy a la pestana de red y copio la ruta.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.32.27.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.39.07.png" alt=""><figcaption></figcaption></figure>

Abro una terminal y con cURL me dirigo a la misma ruta, poniendo las credenciales para que no me devuelva el cartel de <mark style="color:orange;">`Access denied`</mark>. Y obtengo la flag.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 14.44.30.png" alt=""><figcaption></figcaption></figure>











