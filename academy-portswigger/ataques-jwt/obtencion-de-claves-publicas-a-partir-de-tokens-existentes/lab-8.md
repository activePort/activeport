# Lab 8

## <mark style="color:orange;">Omisión de autenticación JWT a través de confusión de algoritmo sin clave expuesta</mark>

<mark style="color:orange;"></mark>

{% embed url="https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion-with-no-exposed-key" %}

Esta práctica de laboratorio usa un mecanismo basado en JWT para manejar sesiones. Utiliza un sólido par de claves RSA para firmar y verificar tokens. Sin embargo, debido a fallas de implementación, este mecanismo es vulnerable a ataques de confusión de algoritmos.

Para resolver la práctica de laboratorio, primero obtenga la clave pública del servidor. Use esta clave para firmar un token de sesión modificado que le da acceso al panel de administración en `/admin`, luego elimine el usuario `carlos`.

Puede iniciar sesión en su propia cuenta con las siguientes credenciales: `wiener:peter`



**Parte 1: obtenga dos JWT generados por el servidor**

1. En Burp, cargue la [extensión JWT ](https://portswigger.net/web-security/jwt)Editor desde la tienda BApp.
2. En el laboratorio, inicie sesión en su propia cuenta y envíe el registro posterior `GET /my-account`solicitud a Burp Repeater.
3. En Burp Repeater, cambie la ruta a `/admin`y enviar la solicitud. Tenga en cuenta que solo se puede acceder al panel de administración cuando se inicia sesión como el `administrator`usuario.
4. Copie su cookie de sesión JWT y guárdela en algún lugar para más adelante.
5. Cerrar sesión y volver a iniciar sesión.
6. Copie la nueva cookie de sesión JWT y guárdela también. Ahora tiene dos JWT válidos generados por el servidor.

**Parte 2: fuerza bruta en la clave pública del servidor**

1.  En una terminal, ejecute el siguiente comando, pasando los dos JWT como argumentos.

    `docker run --rm -it portswigger/sig2n <token1> <token2>`

    Tenga en cuenta que la primera vez que ejecuta esto, pueden pasar varios minutos mientras la imagen se extrae de Docker Hub.
2. Observe que la salida contiene uno o más valores calculados de `n`. Cada uno de estos es matemáticamente posible, pero solo uno de ellos coincide con el valor utilizado por el servidor. En cada caso, la salida también proporciona lo siguiente:
   * Una clave pública codificada en Base64 en formato X.509 y PKCS1.
   * Un JWT manipulado firmado con cada una de estas claves.
3. Copie el JWT alterado de la primera entrada X.509 (es posible que solo tenga uno).
4. Regrese a su solicitud en Burp Repeater y cambie la ruta de regreso a `/my-account`.
5. Reemplace la cookie de sesión con este nuevo JWT y luego envíe la solicitud.
   * Si recibe una respuesta 200 y accede con éxito a la página de su cuenta, entonces esta es la clave X.509 correcta.
   * Si recibe una respuesta 302 que lo redirige a `/login`y elimina su cookie de sesión, entonces esta era la clave X.509 incorrecta. En este caso, repita este paso utilizando el JWT manipulado para cada clave X.509 que generó el script.

**Parte 3: generar una clave de firma maliciosa**

1. Desde la ventana de su terminal, copie la clave X.509 codificada en Base64 que identificó como correcta en la sección anterior. Tenga en cuenta que debe seleccionar la clave, no el JWT manipulado que utilizó en la sección anterior.
2. En Burp, vaya a la **pestaña Claves del editor JWT** y haga clic en **Nueva clave simétrica** .
3. En el cuadro de diálogo, haga clic en **Generar** para generar una nueva clave en formato JWK.
4. Reemplace el valor generado por el `k`propiedad con una clave codificada en Base64 que acaba de copiar. Tenga en cuenta que esta debe ser la clave real, no el JWT manipulado que utilizó en la sección anterior.
5. Guarde la clave.

**Parte 4: modificar y firmar el token**

1. Regrese a su solicitud en Burp Repeater y cambie la ruta a `/admin`.
2. generado por la extensión **Cambie a la pestaña Token web JSON** .
3. En el encabezado del JWT, asegúrese de que el `alg`el parámetro se establece en `HS256`.
4. En la carga útil de JWT, cambie el valor de la `sub`reclamar `administrator`.
5. En la parte inferior de la pestaña, haga clic en **Firmar** y luego seleccione la clave simétrica que generó en la sección anterior.
6. Asegúrese de que la **opción No modificar el encabezado** esté seleccionada y luego haga clic en **Aceptar** . El token modificado ahora se firma utilizando la clave pública del servidor como clave secreta.
7. Envía la solicitud y observa que has accedido con éxito al panel de administración.
8. En la respuesta, busque la URL para eliminar a Carlos ( `/admin/delete?username=carlos`). Envíe la solicitud a este extremo para resolver el laboratorio.

