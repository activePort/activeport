# Obtención de claves públicas a partir de tokens existentes

En los casos en que la clave pública no esté disponible, es posible que aún pueda probar la confusión del algoritmo al derivar la clave de un par de JWT existentes. Este proceso es relativamente simple utilizando herramientas como jwt\_forgery.py. Puede encontrar esto, junto con varios otros scripts útiles, en el [rsa\_sign2n](https://github.com/silentsignal/rsa\_sign2n)[repositorio GitHub ](https://github.com/silentsignal/rsa\_sign2n).

También hemos creado una versión simplificada de esta herramienta, que puede ejecutar como un solo comando:

`docker run --rm -it portswigger/sig2n <token1> <token2>`

{% hint style="info" %}
Necesita la CLI de Docker para ejecutar cualquier versión de la herramienta. La primera vez que ejecute este comando, extraerá automáticamente la imagen de Docker Hub, lo que puede demorar unos minutos.
{% endhint %}

Esto utiliza los JWT que proporciona para calcular uno o más valores potenciales de `n`. No se preocupe demasiado por lo que esto significa: todo lo que necesita saber es que solo uno de estos coincide con el valor de `n`utilizado por la clave del servidor. Para cada valor potencial, nuestro script genera:

* Una clave PEM codificada en Base64 en formato X.509 y PKCS1.
* Un JWT falsificado firmado con cada una de estas claves.

Para identificar la clave correcta, use Burp Repeater para enviar una solicitud que contenga cada uno de los JWT falsificados. Solo uno de estos será aceptado por el servidor. Luego puede usar la clave coincidente para construir un ataque de confusión de algoritmo.
