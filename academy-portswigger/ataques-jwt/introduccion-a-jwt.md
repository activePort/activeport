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



### <mark style="color:orange;">Header (Encabezado)</mark>

El encabezado _generalmente_ consta de dos partes: el tipo de token, que es JWT, y el algoritmo de firma que se utiliza, como HMAC SHA256 o RSA.

Por ejemplo:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Luego, este JSON está codificado en Base64Url para formar la primera parte del JWT.
