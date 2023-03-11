# Lab 6

## <mark style="color:orange;">EVASION DE LA AUTENTICACION JWT A TRAVES DEL RECORRIDO DE LA CABECERA KID</mark>

{% embed url="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones.Para verificar la firma, el servidor utiliza el kidparámetro en[ el encabezado JWT ](https://portswigger.net/web-security/jwt)para obtener la clave relevante de su sistema de archivos. Para resolver el laboratorio, forje un JWT que le dé acceso al panel de administración en /admin, luego elimine el usuario carlos. Puede iniciar sesión en su propia cuenta con las siguientes credenciales: wiener:peter

1 - Inicio sesion.

2 - Capturo el Request con Burpsuite.

3 - Voy a la pestaña JWT Editor Keys y genero una nueva clave simetrica y el valor de k lo reemplazo por `AA==` .

<figure><img src="../../../.gitbook/assets/1 (5) (3).png" alt=""><figcaption></figcaption></figure>

4 - Cambio el valor de kid por la ruta hacia `/dev/null` ,el valor de sub a administrator y la ruta a `/admin` firmo el JWT con la clave que cree anteriormente.

<figure><img src="../../../.gitbook/assets/1 (4).png" alt=""><figcaption></figcaption></figure>

5 - Envio el Request y el response me da OK.

<figure><img src="../../../.gitbook/assets/1 (5).png" alt=""><figcaption></figcaption></figure>

Si el servidor almacena sus claves de verificación en una base de datos, el parámetro kid de encabezado también es un vector potencial para [los ataques de inyección SQL ](https://portswigger.net/web-security/sql-injection).
