# Inyectar JWT autofirmados a traves del parametro JKU

En lugar de incrustar claves públicas directamente usando el jwkparámetro de encabezado, algunos servidores le permiten usar el jku(JWK Set URL) parámetro de encabezado para hacer referencia a un JWK Set que contiene la clave. Al verificar la firma, el servidor obtiene la clave relevante de esta URL.

{% hint style="info" %}
Un JWK Set es un objeto JSON que contiene una matriz de JWK que representan diferentes claves. Puedes ver un ejemplo de esto a continuación.
{% endhint %}

```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
            "n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw-fhvsWQ"
        },
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "d8fDFo-fS9-faS14a9-ASf99sa-7c1Ad5abA",
            "n": "fc3f-yy1wpYmffgXBxhAUJzHql79gNNQ_cb33HocCuJolwDqmk6GPM4Y_qTVX67WhsN3JvaFYw-dfg6DH-asAScw"
        }
    ]
}
```

Los conjuntos JWK como este a veces se exponen públicamente a través de un punto final estándar, como `/.well-known/jwks.json`.

Los sitios web más seguros solo obtendrán claves de dominios confiables, pero a veces puede aprovechar las discrepancias de análisis de URL para evitar este tipo de filtrado. Cubrimos algunos [ejemplos de estos](https://portswigger.net/web-security/ssrf#ssrf-with-whitelist-based-input-filters) en nuestro tema sobre [SSRF](https://portswigger.net/web-security/ssrf) .
