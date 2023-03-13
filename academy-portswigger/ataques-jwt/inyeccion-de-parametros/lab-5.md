# Lab 5

## <mark style="color:orange;">OMISION DE AUTENTICACION A TRAVES DE INYECCION DE ENCABEZADO JKU</mark>

{% embed url="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. El servidor admite la `jku`parámetro en el [encabezado JWT](https://portswigger.net/web-security/jwt) . Sin embargo, no comprueba si la URL proporcionada pertenece a un dominio de confianza antes de obtener la clave. Para resolver el laboratorio, forje un JWT que le dé acceso al panel de administración en `/admin`, luego elimine el usuario `carlos`.

Simplifico los pasos ya que son los mismos a los labs anteriores.

1 - Inicio con credenciales dadas.

2- Capturo Request con Burp.

3 - Cambio url a `/admin` y user a `administrator`.

4 - Genero una nueva clave **RSA Key (si ya tengo generadas no)**

5 - Voy al exploit server

6- Reemplazo el body con

```
{
    "keys": [

    ]
}
```

7 - Copio el RSA Key y la pego dentro de keys.

<figure><img src="../../../.gitbook/assets/1 (2) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1 (30).png" alt=""><figcaption></figcaption></figure>

8 - El valor del kid que agregue al body lo coloco en el kid del token que voy a enviar.

<figure><img src="../../../.gitbook/assets/1 (29).png" alt=""><figcaption></figcaption></figure>

![](<../../../.gitbook/assets/1 (32).png>)

9 - Agrego un nuevo jku parametro en el Header del JWT y su valor sera la URL del servidor de explotacion.

<figure><img src="../../../.gitbook/assets/1 (19).png" alt=""><figcaption></figcaption></figure>

![](<../../../.gitbook/assets/1 (7).png>)

10 - Hago click en firmar y selecciono la clave RSA que genere.

<figure><img src="../../../.gitbook/assets/1 (33).png" alt=""><figcaption></figcaption></figure>

11 - El token modificado ahora esta frimado con la firma correcta. Envio la solicitud.

<figure><img src="../../../.gitbook/assets/1 (9) (1).png" alt=""><figcaption></figcaption></figure>

12 - Y elimino por quinceava vez a carlos.

<figure><img src="../../../.gitbook/assets/1 (17).png" alt=""><figcaption></figcaption></figure>
