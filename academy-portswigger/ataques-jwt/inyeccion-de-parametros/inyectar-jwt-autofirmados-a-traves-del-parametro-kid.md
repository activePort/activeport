# Inyectar JWT autofirmados a traves del parametro KID

Los servidores pueden usar varias claves criptográficas para firmar diferentes tipos de datos, no solo JWT. Por esta razón, el encabezado de un JWT puede contener un `kid`(ID de clave), que ayuda al servidor a identificar qué clave usar al verificar la firma.

Las claves de verificación a menudo se almacenan como un conjunto JWK. En este caso, el servidor puede simplemente buscar el JWK con el mismo `kid`como la ficha.Sin embargo, la especificación JWS no define una estructura concreta para este ID, es solo una cadena arbitraria a elección del desarrollador. Por ejemplo, podrían usar el `kid`parámetro para apuntar a una entrada particular en una base de datos, o incluso el nombre de un archivo.

Si este parámetro también es vulnerable al [cruce de directorios](https://portswigger.net/web-security/file-path-traversal), un atacante podría obligar al servidor a utilizar un archivo arbitrario de su sistema de archivos como clave de verificación.

```json
{
    "kid": "../../path/to/file",
    "typ": "JWT",
    "alg": "HS256",
    "k": "asGsADas3421-dfh9DGN-AFDFDbasfd8-anfjkvc"
}
```

Esto es especialmente peligroso si el servidor también admite JWT firmados mediante un [algoritmo simétrico](https://portswigger.net/web-security/jwt/algorithm-confusion#symmetric-vs-asymmetric-algorithms). En este caso, un atacante podría señalar potencialmente el `kid`parámetro a un archivo estático predecible, luego firme el JWT usando un secreto que coincida con el contenido de este archivo.

En teoría, podría hacer esto con cualquier archivo, pero uno de los métodos más simples es usar `/dev/null`, que está presente en la mayoría de los sistemas Linux. Como este es un archivo vacío, leerlo devuelve una cadena vacía. Por lo tanto, firmar el token con una cadena vacía dará como resultado una firma válida.

{% hint style="info" %}
Si está utilizando la extensión JWT Editor, tenga en cuenta que esto no le permite firmar tokens con una cadena vacía. Sin embargo, debido a un error en la extensión, puede evitar esto utilizando un byte nulo codificado en Base64.
{% endhint %}
