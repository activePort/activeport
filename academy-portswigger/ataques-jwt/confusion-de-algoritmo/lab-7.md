# Lab 7

## <mark style="color:orange;">OMISION DE AUTENTICACION A TRAVES DE CONFUSION DE ALGORITMOS</mark>

{% embed url="https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. Utiliza un sólido par de claves RSA para firmar y verificar tokens. Sin embargo, debido a fallas de implementación, este mecanismo es vulnerable a ataques de confusión de algoritmos.

Para resolver la práctica de laboratorio, primero obtenga la clave pública del servidor. Esto se expone a través de un punto final estándar. Use esta clave para firmar un token de sesión modificado que le da acceso al panel de administración en `/admin`, luego elimine el usuario `carlos`. Puede iniciar sesión en su propia cuenta con las siguientes credenciales:`wiener:peter`

1 - Inicio sesion con las credenciales dadas.

2 - Capturo el request a `/my-account`.&#x20;

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 20.49.29.png" alt=""><figcaption></figcaption></figure>

3 - Cambio la ruta a /admin .

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 20.51.14.png" alt=""><figcaption></figcaption></figure>

4 - En el navegador voy al punto final generico /jwks.json y se ve como el servidor expone el set JWK con la unica clave publica.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 20.57.15.png" alt=""><figcaption></figcaption></figure>

5 - Copio el JWK , genero una nueva clave RSA y pego el JWK. Doy ok para guardar la key.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 20.59.52.png" alt=""><figcaption></figcaption></figure>

6 - Luego copio la Public Key como PEM

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 21.00.37.png" alt=""><figcaption></figcaption></figure>

7 - La pego en el Decoder y la codifico a base64. Copio el resultado.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 21.01.43.png" alt=""><figcaption></figcaption></figure>

8 - Vuelvo a JWT Editor Keys, doy click en Nueva Clave simetrica. Click en generar y el valor de k lo reemplazo por el PEM codificado en Base64. Guardo la clave.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 21.04.12.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 21.04.58.png" alt=""><figcaption></figcaption></figure>

6 - En el Repeater, cambio la ruta a /admin, en el JWT el sub lo cambio a administrator. En el header del JWT cambio el valor del alg a HS256.Hago click en Sign y utilizo la clave simetrica que genere.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 22.13.07.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-10 a la(s) 22.17.02.png" alt=""><figcaption></figcaption></figure>

7 - Mando el request y permite loguear en la pagina del admin.

8 - Borro al usuario Carlos.
