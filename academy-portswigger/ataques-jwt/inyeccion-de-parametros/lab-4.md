# Lab 4

## <mark style="color:orange;">OMISION DE AUTENTICACION MEIDANTE INYECCION DE ENCABEZADO JWK</mark>

{% embed url="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. El servidor admite la jwkparámetro en el [encabezado JWT](https://portswigger.net/web-security/jwt). Esto a veces se usa para incrustar la clave de verificación correcta directamente en el token.Sin embargo, no verifica si la clave provista proviene de una fuente confiable.Para resolver el laboratorio, modifique y firme un JWT que le dé acceso al panel de administración en /admin, luego elimine el usuario carlos. Puede iniciar sesión en su propia cuenta con las siguientes credenciales: wiener:peter

1 - Inicio sesion con las credenciales dadas.

<figure><img src="../../../.gitbook/assets/1 (3).png" alt=""><figcaption></figcaption></figure>

2 - Capturo la solicitud y la envio a Repeater.

<figure><img src="../../../.gitbook/assets/1 (15).png" alt=""><figcaption></figcaption></figure>

3 - Cambio la ruta a admin y el user a administrator, me devuelve que no esta autorizado.

<figure><img src="../../../.gitbook/assets/1 (21).png" alt=""><figcaption></figcaption></figure>

4 - Procedo a realizar una inyeccion de encabezado.

<figure><img src="../../../.gitbook/assets/1 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (18).png" alt=""><figcaption></figcaption></figure>

5 - Envio el Request con el JWT alterado y me devuelve un 200 OK.

<figure><img src="../../../.gitbook/assets/1 (14).png" alt=""><figcaption></figcaption></figure>

6 - Luego borro el usuario Carlos.

<figure><img src="../../../.gitbook/assets/1 (10).png" alt=""><figcaption></figcaption></figure>
