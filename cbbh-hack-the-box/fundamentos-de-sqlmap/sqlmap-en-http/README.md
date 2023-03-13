# SQLMap en HTTP

## <mark style="color:orange;">Ejecutar SQLMap en una solicitud HTTP</mark>

SQLMap tiene numerosas opciones y conmutadores que se pueden usar para configurar correctamente la solicitud (HTTP) antes de su uso.



En muchos casos, errores simples como olvidarse de proporcionar valores de cookies adecuados, complicar demasiado la configuración con una línea de comandos larga o declarar incorrectamente los datos POST formateados, impedirán la detección y explotación correctas de la posible vulnerabilidad de SQLi.



## <mark style="color:orange;">Comandos cURL</mark>

Una de las mejores y más sencillas formas de configurar correctamente una solicitud SQLMap contra el objetivo específico (es decir, solicitud web con parámetros dentro) es utilizando la función <mark style="color:orange;">`Copiar`</mark> como <mark style="color:orange;">`cURL`</mark> desde el panel (Monitor) dentro de las Herramientas para desarrolladores de Chrome, Edge o Firefox:

<figure><img src="../../../.gitbook/assets/1 (10).png" alt=""><figcaption></figcaption></figure>

Al pegar el contenido del portapapeles (<mark style="color:orange;">`Ctrl-V`</mark>) en la línea de comando y cambiando el comando original <mark style="color:orange;">`curl`</mark> a <mark style="color:orange;">`sqlmap`</mark>, podemos usar <mark style="color:orange;">`SQLMap`</mark> con el mismo <mark style="color:orange;">`curl`</mark> dominio:

```bash
activePort@htb[/htb]$ sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```

Cuando se proporcionan datos para su comprobación a <mark style="color:orange;">`SQLMap`</mark>, tiene que haber o bien un valor de parámetro que pueda ser evaluado por vulnerabilidad <mark style="color:orange;">`SQLi`</mark> u opciones/interruptores especializados para la búsqueda automática de parámetros (por ejemplo, <mark style="color:orange;">`--crawl`</mark>, <mark style="color:orange;">`--forms`</mark> o <mark style="color:orange;">`-g`</mark>).



## <mark style="color:orange;">Solicitud GET/POST</mark>

En el escenario más común, los parámetros <mark style="color:orange;">`GET`</mark> se proporcionan con el uso de la opción <mark style="color:orange;">`-u/--url`</mark>, como en el ejemplo anterior. En cuanto a la comprobación de datos <mark style="color:orange;">`POST`</mark>, se puede utilizar el indicador <mark style="color:orange;">`--data`</mark>, como se indica a continuación:

```bash
activePort@htb[/htb]$ sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```

En tales casos, los parámetros <mark style="color:orange;">`POST`</mark> <mark style="color:blue;">uid</mark> y <mark style="color:blue;">name</mark> serán probados para vulnerabilidad <mark style="color:orange;">`SQLi`</mark>. Por ejemplo, si tenemos una indicación clara de que el parámetro <mark style="color:blue;">uid</mark> es propenso a una vulnerabilidad <mark style="color:orange;">`SQLi`</mark>, podríamos limitar las pruebas a sólo este parámetro utilizando <mark style="color:orange;">-p</mark> <mark style="color:blue;">uid</mark>. De lo contrario, podríamos marcarlo dentro de los datos proporcionados con el uso del marcador especial <mark style="color:orange;">`*`</mark> como se indica a continuación:

```bash
p314d0@htb[/htb]$ sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```



## <mark style="color:orange;">Solicitudes HTTP completas</mark>

Si necesitamos especificar una petición <mark style="color:orange;">`HTTP`</mark> compleja con muchos valores de cabecera diferentes y un cuerpo <mark style="color:orange;">`POST`</mark> alargado, podemos utilizar el indicador <mark style="color:orange;">`-r`</mark>. Con esta opción, <mark style="color:orange;">`SQLMap`</mark> recibe el "archivo de solicitud", que contiene toda la solicitud <mark style="color:orange;">`HTTP`</mark> en un único archivo de texto. En un escenario común, dicha solicitud HTTP puede ser capturada desde una aplicación proxy especializada (por ejemplo, <mark style="color:orange;">Burp</mark>) y escrita en el archivo de solicitud, de la siguiente manera:

<figure><img src="../../../.gitbook/assets/2 (1).png" alt=""><figcaption></figcaption></figure>

Un ejemplo de una solicitud HTTP capturada con `Burp`se vería como:

```http
GET /?id=1 HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
DNT: 1
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT
If-None-Match: "3147526947"
Cache-Control: max-age=0
```

Podemos copiar manualmente la solicitud <mark style="color:orange;">`HTTP`</mark> desde dentro <mark style="color:orange;">`Burp`</mark> y escribirlo en un archivo, o podemos hacer clic derecho en la solicitud dentro <mark style="color:orange;">`Burp`</mark> y elige <mark style="color:orange;">`Copy to file`</mark>. Otra forma de capturar la solicitud <mark style="color:orange;">`HTTP`</mark> completa sería usar el navegador, como se mencionó anteriormente en la sección, y elegir la opción <mark style="color:orange;">`Copy > Copy Request Headers`</mark> <mark style="color:orange;"></mark><mark style="color:orange;"></mark> y luego pegar la solicitud en un archivo.

Para ejecutar <mark style="color:orange;">`SQLMap`</mark> con un archivo de solicitud <mark style="color:orange;">`HTTP`</mark>, usamos el <mark style="color:orange;">`-r`</mark> bandera, de la siguiente manera:

```bash
activePort@htb[/htb]$ sqlmap -r req.txt
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.9}
|_ -| . [(]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org


[*] starting @ 14:32:59 /2020-09-11/

[14:32:59] [INFO] parsing HTTP request from 'req.txt'
[14:32:59] [INFO] testing connection to the target URL
[14:32:59] [INFO] testing if the target URL content is stable
[14:33:00] [INFO] target URL content is stable
```

{% hint style="info" %}
De manera similar al caso con la opción <mark style="color:orange;">`--data`</mark>, dentro del archivo de solicitud guardado, podemos especificar el parámetro que queremos inyectar con un asterisco (_), como '/?id=_'.
{% endhint %}



## <mark style="color:orange;">Solicitudes personalizadas de SQLMap</mark>

Si quisiéramos elaborar manualmente solicitudes complicadas, existen numerosos modificadores y opciones para ajustar SQLMap.

Si quisiéramos elaborar manualmente solicitudes complicadas, existen numerosos modificadores y opciones para ajustar <mark style="color:orange;">`SQLMap`</mark>. Por ejemplo, si hay un requisito para especificar el valor de la cookie (sesión) para <mark style="color:orange;">`PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c`</mark> opción <mark style="color:orange;">`--cookie`</mark> se usaría de la siguiente manera:

```bash
activePort@htb[/htb]$ sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

El mismo efecto se puede hacer con el uso de la opción <mark style="color:orange;">`-H/--header`</mark>:

```bash
activePort@htb[/htb]$ sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

Podemos aplicar lo mismo a opciones como <mark style="color:orange;">`--host`</mark>, <mark style="color:orange;">`--referer`</mark>, y <mark style="color:orange;">`-A/--user-agent`</mark>, que se utilizan para especificar los mismos valores de encabezados <mark style="color:orange;">`HTTP`</mark>.



Además, hay un modificador <mark style="color:orange;">`--random-agent`</mark> diseñado para seleccionar aleatoriamente un valor de cabecera <mark style="color:orange;">`User-agent`</mark> de la base de datos incluida de valores de navegador habituales. Es importante recordar este parámetro, ya que cada vez más soluciones de protección eliminan automáticamente todo el tráfico <mark style="color:orange;">`HTTP`</mark> que contenga el valor <mark style="color:orange;">`User-agent`</mark> predeterminado reconocible de SQLMap (por ejemplo, `User-agent: sqlmap/1.4.9.12#dev` ([http://sqlmap.org](http://sqlmap.org/))). Alternativamente, se puede utilizar el modificador <mark style="color:orange;">`--mobile`</mark> para imitar al smartphone utilizando ese mismo valor de cabecera.

Aunque SQLMap, por defecto, sólo se centra en los parámetros <mark style="color:orange;">`HTTP`</mark>, es posible comprobar las cabeceras para detectar la vulnerabilidad <mark style="color:orange;">`SQLi`</mark>. La forma más sencilla es especificar la marca de inyección "personalizada" después del valor de la cabecera (por ejemplo, <mark style="color:orange;">`--cookie="id=1*"`</mark>). El mismo principio se aplica a cualquier otra parte de la petición.

Además, si quisiéramos especificar un método <mark style="color:orange;">HTTP</mark> alternativo, distinto de <mark style="color:orange;">`GET`</mark> y <mark style="color:orange;">`POST`</mark> (por ejemplo, <mark style="color:orange;">`PUT`</mark>), podemos utilizar la opción <mark style="color:orange;">`--method`</mark>, de la siguiente manera:

```bash
activePort@htb[/htb]$ sqlmap -u www.target.com --data='id=1' --method PUT
```



## <mark style="color:orange;">Solicitudes HTTP personalizadas</mark>

Aparte del estilo de cuerpo <mark style="color:orange;">`POST`</mark> de datos de formulario más común (por ejemplo, <mark style="color:orange;">`id=1`</mark>), SQLMap también admite solicitudes HTTP con formato JSON (por ejemplo, <mark style="color:orange;">`{"id":1}`</mark>) y XML (por ejemplo, <mark style="color:orange;">`<element><id>1</id></element>`</mark>).

La compatibilidad con estos formatos se implementa de forma "relajada"; por lo tanto, no existen restricciones estrictas sobre cómo se almacenan los valores de los parámetros en su interior. En caso de que el cuerpo del POST sea relativamente simple y corto, la opción <mark style="color:orange;">`--data`</mark> será suficiente.

Sin embargo, en el caso de un cuerpo POST complejo o largo, podemos utilizar de nuevo la opción <mark style="color:orange;">`-r`</mark>:

```bash
activePort@htb[/htb]$ cat req.txt
HTTP / HTTP/1.0
Host: www.example.com

{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Example JSON",
      "body": "Just an example",
      "created": "2020-05-22T14:56:29.000Z",
      "updated": "2020-05-22T14:56:28.000Z"
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "user"}
      }
    }
  }]
}
```

```bash
activePort@htb[/htb]$ sqlmap -r req.txt
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.9}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org


[*] starting @ 00:03:44 /2020-09-15/

[00:03:44] [INFO] parsing HTTP request from 'req.txt'
JSON data found in HTTP body. Do you want to process it? [Y/n/q] 
[00:03:45] [INFO] testing connection to the target URL
[00:03:45] [INFO] testing if the target URL content is stable
[00:03:46] [INFO] testing if HTTP parameter 'JSON type' is dynamic
[00:03:46] [WARNING] HTTP parameter 'JSON type' does not appear to be dynamic
[00:03:46] [WARNING] heuristic (basic) test shows that HTTP parameter 'JSON type' might not be injectable
```
