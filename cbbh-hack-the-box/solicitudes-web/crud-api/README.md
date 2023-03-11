---
cover: >-
  https://images.unsplash.com/photo-1634633111555-48907980b902?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw3fHxhcGl8ZW58MHx8fHwxNjc4NTYzMDQz&ixlib=rb-4.0.3&q=80
coverY: 0
---

# CRUD API

Vimos ejemplos de la aplicacion web City Search que utiliza parámetros de PHP para buscar el nombre de una ciudad en las secciones anteriores. Esta sección analizará cómo una aplicación web de este tipo puede utilizar las API para realizar lo mismo, e interactuaremos directamente con el punto final de la API.



## <mark style="color:orange;">API</mark>

Hay varios tipos de API. Muchas API se utilizan para interactuar con una base de datos, de modo que podamos especificar la tabla solicitada y la fila solicitada dentro de nuestra consulta API, y luego usar un método HTTP para realizar la operación necesaria. Por ejemplo, para el endpoint <mark style="color:orange;">`api.php`</mark> en nuestro ejemplo, si quisiéramos actualizar la tabla <mark style="color:orange;">`city`</mark> en la base de datos, y la fila que actualizaremos tiene un nombre de ciudad de london, entonces la URL se vería así:

```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...SNIP...
```



## <mark style="color:orange;">CRUD</mark>

Como podemos ver, podemos especificar fácilmente la tabla y la fila en la que queremos realizar una operación a través de dichas API. Entonces podemos utilizar diferentes métodos HTTP para realizar diferentes operaciones en esa fila. En general, las API realizan 4 operaciones principales en la entidad de la base de datos solicitada:

| Operación | Método HTTP | Descripción                                                    |
| --------- | ----------- | -------------------------------------------------------------- |
| `Create`  | `POST`      | Agrega los datos especificados a la tabla de la base de datos. |
| `Read`    | `GET`       | Lee la entidad especificada de la tabla de la base de datos    |
| `Update`  | `PUT`       | Actualiza los datos de la tabla de base de datos especificada  |
| `Delete`  | `DELETE`    | Elimina la fila especificada de la tabla de la base de datos   |

Estas cuatro operaciones están vinculadas principalmente a las API CRUD comúnmente conocidas, pero el mismo principio también se usa en las API REST y varios otros tipos de API. Por supuesto, no todas las API funcionan de la misma manera, y el control de acceso de los usuarios limitará qué acciones podemos realizar y qué resultados podemos ver. El módulo Introducción a las aplicaciones web explica con más detalle estos conceptos, por lo que puede consultarlo para obtener más detalles sobre las API y su uso.



## <mark style="color:orange;">Read</mark>

Lo primero que haremos al interactuar con una API es leer datos. Como se mencionó anteriormente, simplemente podemos especificar el nombre de la tabla después de la API (p. ej. <mark style="color:orange;">/city</mark>) y luego especifique nuestro término de búsqueda (p. ej. <mark style="color:orange;">/london</mark>), como sigue:

```bash
p314d0@htb[/htb]$ curl http://<SERVER_IP>:<PORT>/api.php/city/london

[{"city_name":"London","country_name":"(UK)"}]
```

Vemos que el resultado se envía como una cadena JSON. Para tenerlo correctamente formateado en formato JSON, podemos canalizar la salida al <mark style="color:orange;">`jq`</mark> utilidad, que lo formateará correctamente. También silenciaremos cualquier salida cURL innecesaria con <mark style="color:orange;">`-s`</mark>, como sigue:

```bash
p314d0@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  }
]
```

Como podemos ver, obtuvimos la salida en una salida bien formateada. También podemos proporcionar un término de búsqueda y obtener todos los resultados coincidentes:

```bash
p314d0@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/le | jq

[
  {
    "city_name": "Leeds",
    "country_name": "(UK)"
  },
  {
    "city_name": "Dudley",
    "country_name": "(UK)"
  },
  {
    "city_name": "Leicester",
    "country_name": "(UK)"
  },
  ...SNIP...
]
```

Finalmente, podemos pasar una cadena vacía para recuperar todos los enteros en la tabla:

```bash
p314d0@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  },
  {
    "city_name": "Birmingham",
    "country_name": "(UK)"
  },
  {
    "city_name": "Leeds",
    "country_name": "(UK)"
  },
  ...SNIP...
]
```



## <mark style="color:orange;">Create</mark>

Para agregar una nueva entrada, podemos usar una solicitud <mark style="color:orange;">`HTTP POST`</mark>, que es bastante similar a lo que hemos realizado en la sección anterior. Simplemente podemos <mark style="color:orange;">`PUBLICAR`</mark> nuestros datos <mark style="color:orange;">`JSON`</mark> y se agregarán a la tabla. Como esta API usa datos JSON, también estableceremos el <mark style="color:orange;">`Content-Type`</mark>  encabezado a JSON, de la siguiente manera:

```bash
p314d0@htb[/htb]$ curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

Ahora, podemos leer el contenido de la ciudad que agregamos ( <mark style="color:orange;">`HTB_City`</mark>), para ver si se agregó con éxito:

```bash
p314d0@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq

[
  {
    "city_name": "HTB_City",
    "country_name": "HTB"
  }
]
```

Como podemos ver, se creó una nueva ciudad, que antes no existía.

{% hint style="info" %}
Intente agregar una nueva ciudad a través de las herramientas de desarrollo del navegador, usando una de las solicitudes Fetch POST que usó en la sección anterior.
{% endhint %}



## <mark style="color:orange;">Update</mark>

Ahora que sabemos cómo leer y escribir entradas a través de las API, comencemos a discutir otros dos métodos HTTP que no hemos usado hasta ahora: PUTy DELETE. Como se mencionó al principio de la sección, PUTse utiliza para actualizar las entradas de la API y modificar sus detalles, mientras que DELETEse utiliza para eliminar una entidad específica.

{% hint style="info" %}
El metodo HTTP PATCH también se puede usar para actualizar las entradas de la API en lugar de PUT. Para ser preciso, PATCHse utiliza para actualizar parcialmente una entrada (solo modificar algunos de sus datos "por ejemplo, solo city\_name"), mientras que PUTse utiliza para actualizar toda la entrada. También podemos usar el HTTP OPTIONSpara ver cuál de los dos es aceptado por el servidor y, a continuación, utilice el método adecuado en consecuencia. En esta sección, nos centraremos en la PUTmétodo, aunque su uso es bastante similar.
{% endhint %}

Usando `PUT` es bastante similar a `POST` en este caso, con la única diferencia de que tenemos que especificar el nombre de la entidad que queremos editar en la `URL`, de lo contrario, la `API` no sabrá qué entidad editar. Entonces, todo lo que tenemos que hacer es especificar el `city` nombre en la `URL`, cambie el método de solicitud a `PUT` y proporcione los datos `JSON` como lo hicimos con `POST`, de la siguiente manera:

```bash
activePort@htb[/htb]$ curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

Vemos en el ejemplo anterior que primero especificamos <mark style="color:orange;">`/city/london`</mark> como nuestra ciudad, y pasó una cadena <mark style="color:orange;">`JSON`</mark> que contenía <mark style="color:orange;">`"city_name":"New_HTB_City"`</mark> en los datos de la solicitud. Entonces, la ciudad de Londres ya no debería existir, y una nueva ciudad con el nombre New\_HTB\_City debería existir. Intentemos leer ambos para confirmar:

```bash
activePort@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```

```shell-session
activePort@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq

[
  {
    "city_name": "New_HTB_City",
    "country_name": "HTB"
  }
]
```

De hecho, reemplazamos con éxito el nombre de la ciudad antigua con el de la ciudad nueva.

{% hint style="info" %}
En algunas API, el `Update`La operación también se puede utilizar para crear nuevas entradas. Básicamente, enviaríamos nuestros datos, y si no existen, los crearía. Por ejemplo, en el ejemplo anterior, incluso si una entrada con un `london`ciudad no existiera, se crearía una nueva entrada con los datos que pasamos. En nuestro ejemplo, sin embargo, este no es el caso. Intente actualizar una ciudad inexistente y vea lo que obtiene.
{% endhint %}



## <mark style="color:orange;">Delete</mark>

Finalmente, intentemos borrar una ciudad, que es tan fácil como leer una ciudad. Simplemente especificamos el nombre de la ciudad para la <mark style="color:orange;">`API`</mark> y usamos el <mark style="color:orange;">`HTTP DELETE`</mark> método, y eliminaría la entrada, de la siguiente manera:

```bash
activePort@htb[/htb]$ curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

```bash
activePort@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
[]
```

Como podemos ver, después de eliminar `New_HTB_City`, obtenemos una matriz vacía cuando intentamos leerla, lo que significa que ya no existe.

{% hint style="info" %}
Intente eliminar cualquiera de las ciudades que agregó anteriormente a través de solicitudes POST y luego lea todas las entradas para confirmar que se eliminaron correctamente.
{% endhint %}

Con esto, podemos realizar los 4 operaciones CRUD a través de cURL. En una aplicación web real, es posible que tales acciones no estén permitidas para todos los usuarios, o se consideraría una vulnerabilidad si alguien puede modificar o eliminar cualquier entrada. Cada usuario tendría ciertos privilegios sobre lo que puede reado write, dónde write se refiere a agregar, modificar o eliminar datos. Para autenticar a nuestro usuario para usar la API, necesitaríamos pasar una cookie o un encabezado de autorización (por ejemplo, JWT), como hicimos en una sección anterior. Aparte de eso, las operaciones son similares a las que practicamos en esta sección.

