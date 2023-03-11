# Lab 8

## <mark style="color:orange;">Omisión de autenticación JWT a través de confusión de algoritmo sin clave expuesta</mark>

<mark style="color:orange;"></mark>

{% embed url="https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion-with-no-exposed-key" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. Utiliza un sólido par de claves RSA para firmar y verificar tokens. Sin embargo, debido a fallas de implementación, este mecanismo es vulnerable a ataques de confusión de algoritmos.

Para resolver la práctica de laboratorio, primero obtenga la clave pública del servidor. Use esta clave para firmar un token de sesión modificado que le da acceso al panel de administración en `/admin`, luego elimine el usuario `carlos`.

Puede iniciar sesión en su propia cuenta con las siguientes credenciales: `wiener:peter`

1 - Inicio sesion con las credenciales dadas.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 07.42.22.png" alt=""><figcaption></figcaption></figure>

2 - Capturo el Request con Burpsuite y lo envio a Repeater.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2023-03-11 a la(s) 07.42.15.png" alt=""><figcaption></figcaption></figure>

3 -&#x20;
