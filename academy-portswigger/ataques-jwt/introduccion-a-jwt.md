# Introduccion a JWT

## <mark style="color:orange;">¿Que es el JSON Web Token?</mark>

El JSON Web Token (JWT) es un estándar abierto ([RFC 7519](https://tools.ietf.org/html/rfc7519)) que define una forma compacta y autónoma de transmitir información de forma segura entre las partes como un objeto JSON. Esta información se puede verificar y confiar porque está firmada digitalmente. Los JWT se pueden firmar usando un secreto (con el **algoritmo HMAC**) o un par de claves pública/privada usando **RSA** o **ECDSA .**

Aunque los JWT se pueden cifrar para proporcionar también confidencialidad entre las partes, nos centraremos en los _tokens firmados_. Los tokens firmados pueden verificar la _integridad_ de los reclamos contenidos en ellos, mientras que los tokens encriptados _ocultan_ esos reclamos de otras partes. Cuando los tokens se firman utilizando pares de claves pública/privada, la firma también certifica que solo la parte que posee la clave privada es la que la firmó.



## <mark style="color:orange;">¿Cuándo debería usar JSON Web Tokens?</mark>

Estos son algunos escenarios en los que los JSON Web Token son útiles:

* _**Autorización**_ : este es el escenario más común para usar JWT. Una vez que el usuario haya iniciado sesión, cada solicitud posterior incluirá el JWT, lo que permitirá al usuario acceder a rutas, servicios y recursos permitidos con ese token. El inicio de sesión único es una función que se usa ampliamente en JWT en la actualidad, debido a su pequeña sobrecarga y su capacidad para usarse fácilmente en diferentes dominios.
* _**Intercambio de información**_ : los JWT son una buena manera de transmitir información de forma segura entre las partes. Debido a que los JWT se pueden firmar, por ejemplo, utilizando pares de claves pública/privada, puede estar seguro de que los remitentes son quienes dicen ser. Además, como la firma se calcula utilizando el encabezado y la carga útil, también puede verificar que el contenido no haya sido alterado.



## <mark style="color:orange;">¿Qué es la estructura del token web JSON?</mark>

En su forma compacta, los tokens JWT constan de tres partes separadas por puntos ( . ), que son:

* <mark style="color:red;">Encabezamiento</mark>
* <mark style="color:yellow;">Carga útil</mark>
* <mark style="color:green;">Firma</mark>

Por lo tanto, un JWT normalmente tiene el siguiente aspecto.

<mark style="color:red;">`xxxxx`</mark>`.`<mark style="color:yellow;">`yyyyy`</mark>`.`<mark style="color:green;">`zzzzz`</mark>

Vamos a desglosar las diferentes partes.



## <mark style="color:orange;">Header (Encabezado)</mark>

El encabezado _generalmente_ consta de dos partes: el tipo de token, que es JWT, y el algoritmo de firma que se utiliza, como HMAC SHA256 o RSA.

Por ejemplo:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Luego, este JSON está codificado en Base64Url para formar la primera parte del JWT.



## <mark style="color:orange;">Payload (Carga util)</mark>

La segunda parte del token es la carga útil, que contiene las reclamaciones. Las reclamaciones son declaraciones sobre una entidad (normalmente, el usuario) y datos adicionales. Hay tres tipos de reclamos: _registrados_, _públicos_ y _privados_.

* [**Reclamos registrados**](https://tools.ietf.org/html/rfc7519#section-4.1) : se trata de un conjunto de reclamos predefinidos que no son obligatorios pero se recomiendan para proporcionar un conjunto de reclamos útiles e interoperables. Algunos de ellos son: **`iss`** (emisor), **`exp`** (tiempo de caducidad), **`sub`** (sujeto), **`aud`** (audiencia), entre [otros](https://tools.ietf.org/html/rfc7519#section-4.1) .

{% hint style="info" %}
Tenga en cuenta que los nombres de los reclamos tienen solo tres caracteres, ya que JWT debe ser compacto.
{% endhint %}

* [**Reclamos públicos**](https://tools.ietf.org/html/rfc7519#section-4.2) : estos pueden ser definidos a voluntad por aquellos que usan JWT. Pero para evitar colisiones, deben definirse en el [Registro de tokens web JSON de IANA](https://www.iana.org/assignments/jwt/jwt.xhtml) o definirse como un URI que contenga un espacio de nombres resistente a colisiones.
* [**Reclamos privados**](https://tools.ietf.org/html/rfc7519#section-4.3) : Son los reclamos personalizados creados para compartir información entre partes que acuerdan usarlos y no son _reclamos registrados_ ni _reclamos públicos ._ reclamos.

Un ejemplo de Payload podria ser:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

Luego, la carga útil se codifica en Base64Url para formar la segunda parte del token web JSON.

{% hint style="info" %}
Tenga en cuenta que para los tokens firmados esta información, aunque está protegida contra la manipulación, cualquiera puede leerla. No coloque información secreta en la carga útil o en los elementos del encabezado de un JWT a menos que esté encriptada.
{% endhint %}



## <mark style="color:orange;">Signature (Firma)</mark>

Para crear la parte de la firma, debe tomar el encabezado codificado, la carga útil codificada, un secreto, el algoritmo especificado en el encabezado y firmarlo.

Por ejemplo, si desea utilizar el algoritmo HMAC SHA256, la firma se creará de la siguiente manera:

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

La firma se usa para verificar que el mensaje no se modificó en el camino y, en el caso de tokens firmados con una clave privada, también puede verificar que el remitente del JWT es quien dice ser.



## <mark style="color:orange;">Poniendo todo junto</mark>

El resultado son tres cadenas de URL Base64 separadas por puntos que se pueden pasar fácilmente en entornos HTML y HTTP, mientras que son más compactas en comparación con los estándares basados en XML, como SAML.

A continuación, se muestra un JWT que tiene codificados el encabezado y la carga útiles anteriores, y está firmado con un secreto.

<mark style="color:red;">eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9</mark>.<mark style="color:yellow;">eyJleHAiOjE2NzgzNjk0MjksImRhdGEiOnsiZW1haWwiOiJwZW50ZXN0aW5nMjAzMkBnbWFpbC5jb20iLCJ1c2VySWQiOiIwN2Y2MjYzZi1mMzNkLTQzZTUtYTNhMS03MjQyNTNhMTcwZWEifSwiaWF0IjoxNjc4MjgzMDI5fQ</mark>.<mark style="color:green;">blyWcb6xchZ3TTqqH2N1C6MKgRXnUrq8-wbM6dpi7J8</mark>

Si quiere jugar con JWT y poner en práctica estos conceptos, puede usar [jwt.io Debugger ](https://jwt.io/#debugger-io)para decodificar, verificar y generar JWT.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2023-03-08 a la(s) 11.46.25.png" alt=""><figcaption></figcaption></figure>
