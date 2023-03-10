# Lab 3

{% embed url="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. Utiliza una clave secreta extremadamente débil para firmar y verificar tokens.Esto se puede forzar fácilmente usando una [lista de palabras de secretos comunes](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list). Para resolver el laboratorio, primero fuerza bruta la clave secreta del sitio web.Una vez que haya obtenido esto, utilícelo para firmar un token de sesión modificado que le da acceso al panel de administración en `/admin`, luego elimine el usuario `carlos`. Puede iniciar sesión en su propia cuenta con las siguientes credenciales: `wiener:peter`&#x20;

1 - Inicio sesion

<figure><img src="../../../.gitbook/assets/1 (31).png" alt=""><figcaption></figcaption></figure>

2 - Capturo el Request y lo envio al Repeater.

<figure><img src="../../../.gitbook/assets/1 (12).png" alt=""><figcaption></figcaption></figure>

3 - Copio la cookie y le aplico fuerza bruta para obtener la clave secreta.

<figure><img src="../../../.gitbook/assets/1 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (16).png" alt=""><figcaption></figcaption></figure>

4 - En Burpsuite, en la pestaña Decoder, codifico el secreto en Base64 que forze con Hashcat.

<figure><img src="../../../.gitbook/assets/1 (10).png" alt=""><figcaption></figcaption></figure>

5 - Luego, voy al JWT Editor Keys. Clickeo en New Symmetric Key y genero una nueva clave. El valor de k lo reemplazo por el secreto que codifique en Base64. Doy click en OK para guardar la clave.

<figure><img src="../../../.gitbook/assets/1 (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (13).png" alt=""><figcaption></figcaption></figure>

6 - Vuelvo al Repeater y a la pestaña JSON Web Token. Cambio `"sub : wiener"` por `"sub : administrator"` y luego click en sign (firmar). Selecciono la clave que genere anteriromente. `DONT MODIFY HEADER` desbe estar seleccionado.

<figure><img src="../../../.gitbook/assets/1 (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (15).png" alt=""><figcaption></figcaption></figure>

7 - Envio el Request con el token modificado con la firma correcta. Y procedo a borrar al usuario Carlos.

<figure><img src="../../../.gitbook/assets/1 (25).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (26).png" alt=""><figcaption></figcaption></figure>

Si el servidor usa un secreto extremadamente débil, incluso puede ser posible aplicar fuerza bruta a este carácter por carácter en lugar de usar una lista de palabras.
