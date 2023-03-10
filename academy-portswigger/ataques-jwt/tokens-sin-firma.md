# Tokens sin firma

Entre otras cosas, el encabezado JWT contiene un parámetro alg .Esto le dice al servidor qué algoritmo se usó para firmar el token y, por lo tanto, qué algoritmo debe usar al verificar la firma.

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

Esto es inherentemente defectuoso porque el servidor no tiene otra opción que confiar implícitamente en la entrada controlable por el usuario del token que, en este punto, no se ha verificado en absoluto. En otras palabras, un atacante puede influir directamente en cómo el servidor verifica si el token es confiable.



Los JWT se pueden firmar utilizando una variedad de algoritmos diferentes, pero también se pueden dejar sin firmar. En este caso, el parámetro alg se establece en none, que indica un llamado "JWT no seguro". Debido a los peligros obvios de esto, los servidores generalmente rechazan tokens sin firma. Sin embargo, como este tipo de filtrado se basa en el análisis de cadenas, a veces puede omitir estos filtros utilizando técnicas de ofuscación clásicas, como mayúsculas y codificaciones inesperadas.

{% hint style="warning" %}
Incluso si el token no está firmado, la parte de la carga útil aún debe terminar con un punto final.
{% endhint %}







