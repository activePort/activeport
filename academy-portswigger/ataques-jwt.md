---
cover: ../.gitbook/assets/jwt.svg
coverY: 0
---

# Ataques JWT

En esta sección, veremos cómo los problemas de diseño y el manejo defectuoso de los tokens web JSON (JWT) pueden hacer que los sitios web sean vulnerables a una variedad de ataques de alta gravedad. Dado que los JWT se utilizan con mayor frecuencia en la autenticación, la gestión de sesiones y [los mecanismos de control de acceso ](https://portswigger.net/web-security/access-control), estas vulnerabilidades pueden comprometer potencialmente todo el sitio web y sus usuarios.

<figure><img src="../.gitbook/assets/jwt-infographic.jpg" alt=""><figcaption></figcaption></figure>

### <mark style="color:orange;">INTRODUCCION A LOS TOKENS WEB JSON</mark>

#### ¿Que es el token web JSON ?

El JSON Web Token (JWT) es un estándar abierto ([RFC 7519](https://tools.ietf.org/html/rfc7519)) que define una forma compacta y autónoma de transmitir información de forma segura entre las partes como un objeto JSON. Esta información se puede verificar y confiar porque está firmada digitalmente. Los JWT se pueden firmar usando un secreto (con el **algoritmo HMAC**) o un par de claves pública/privada usando **RSA** o **ECDSA .**

Aunque los JWT se pueden cifrar para proporcionar también confidencialidad entre las partes, nos centraremos en los _tokens firmados_ . Los tokens firmados pueden verificar la _integridad_ de los reclamos contenidos en ellos, mientras que los tokens encriptados _ocultan_ esos reclamos de otras partes. Cuando los tokens se firman utilizando pares de claves pública/privada, la firma también certifica que solo la parte que posee la clave privada es la que la firmó.
