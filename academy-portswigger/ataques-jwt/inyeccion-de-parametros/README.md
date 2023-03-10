# Inyeccion de parametros

## <mark style="color:orange;">INYECCION DE PARAMETROS EN ENCABEZADO JWT</mark>&#x20;

De acuerdo con la especificación JWS, sólo el `alg` parámetro de encabezado es obligatorio. Sin embargo, en la práctica, los encabezados JWT (también conocidos como encabezados JOSE) a menudo contienen otros parámetros.Los siguientes son de particular interés para los atacantes.

* `jwk`(JSON Web Key): proporciona un objeto JSON incrustado que representa la clave.
* `jku`(URL del conjunto de claves web JSON): proporciona una URL desde la cual los servidores pueden obtener un conjunto de claves que contienen la clave correcta.
* `kid`(ID de clave): proporciona una ID que los servidores pueden usar para identificar la clave correcta en los casos en que hay varias claves para elegir. Dependiendo del formato de la clave, esta puede tener una coincidencia `kid`parámetro.

Como puede ver, cada uno de estos parámetros controlables por el usuario le dice al servidor del destinatario qué clave usar al verificar la firma. En esta sección, aprenderá cómo explotarlos para inyectar JWT modificados firmados con su propia clave arbitraria en lugar del secreto del servidor.



## <mark style="color:orange;">INYECTAR JWT AUTOFIRMADOS A TRAVES DEL PARAMETRO JWK</mark>

La especificación JSON Web Signature (JWS) describe una opción jwkparámetro de encabezado, que los servidores pueden usar para incrustar su clave pública directamente dentro del token en formato JWK.

{% hint style="info" %}
Un JWK (JSON Web Key) es un formato estandarizado para representar claves como un objeto JSON.
{% endhint %}

Puede ver un ejemplo de esto en el siguiente encabezado JWT:

```json
{
    "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
    "typ": "JWT",
    "alg": "RS256",
    "jwk": {
        "kty": "RSA",
        "e": "AQAB",
        "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
        "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m"
    }
}
```

{% hint style="info" %}
En caso de que no esté familiarizado con los términos "clave pública" y "clave privada", hemos cubierto esto como parte de nuestros materiales sobre ataques de confusión de algoritmos. Para obtener más información, consulte [Algoritmos simétricos frente a asimétricos ](https://portswigger.net/web-security/jwt/algorithm-confusion#symmetric-vs-asymmetric-algorithms).
{% endhint %}

Idealmente, los servidores solo deberían usar una lista blanca limitada de claves públicas para verificar las firmas JWT. Sin embargo, los servidores mal configurados a veces usan cualquier clave que esté incrustada en el `jwk`parámetro.

Puede aprovechar este comportamiento firmando un JWT modificado con su propia clave privada RSA y luego incrustando la clave pública coincidente en el `jwk`encabezamiento.

Aunque puede agregar o modificar manualmente el `jwk`en Burp, la [extensión JWT Editor](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) proporciona una característica útil para ayudarlo a probar esta vulnerabilidad:

1. Con la extensión cargada, en la barra de pestañas principal de Burp, vaya a la pestaña **JWT EDITOR KEYS** .
2. [Genere una nueva clave RSA.](https://portswigger.net/web-security/jwt/working-with-jwts-in-burp-suite#adding-new-signing-keys)
3. Envíe una solicitud que contenga un JWT a Burp Repeater.
4. generado por la extensión **Token web JSON** En el editor de mensajes, cambie a la pestaña [y modifique](https://portswigger.net/web-security/jwt/working-with-jwts-in-burp-suite#editing-the-contents-of-jwts) la carga útil del token como desee.
5. Haga clic en **Ataque** , luego seleccione **Embedded JWK** . Cuando se le solicite, seleccione su clave RSA recién generada.
6. Envíe la solicitud para probar cómo responde el servidor.

También puede realizar este ataque manualmente agregando el `jwk`encabezado usted mismo. Sin embargo, es posible que también deba actualizar los JWT `kid`parámetro de encabezado para que coincida con el `kid`de la clave incrustada. El ataque incorporado de la extensión se encarga de este paso por usted.
