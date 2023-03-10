# Fuerza bruta

## <mark style="color:orange;">Forzar las claves secretas</mark>

Algunos algoritmos de firma, como HS256 (HMAC + SHA-256), utilizan una cadena independiente arbitraria como clave secreta. Al igual que una contraseña, es crucial que un atacante no pueda adivinar fácilmente este secreto o forzarlo por la fuerza bruta. De lo contrario, es posible que puedan crear JWT con cualquier encabezado y valores de carga útil que deseen, y luego usar la clave para volver a firmar el token con una firma válida.

Al implementar aplicaciones JWT, los desarrolladores a veces cometen errores como olvidarse de cambiar los secretos predeterminados o de marcador de posición.Incluso pueden copiar y pegar fragmentos de código que encuentran en línea y luego olvidarse de cambiar un secreto codificado que se proporciona como ejemplo. En este caso, puede ser trivial para un atacante aplicar fuerza bruta al secreto de un servidor utilizando una [lista de palabras de secretos conocidos](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list).



## <mark style="color:orange;">Forzar claves secretas usando Hashcat</mark>

Recomendamos usar hashcat para aplicar fuerza bruta a las claves secretas. Puede [instalar hashcat manualmente](https://hashcat.net/wiki/doku.php?id=frequently\_asked\_questions#how\_do\_i\_install\_hashcat) , pero también viene preinstalado y listo para usar en Kali Linux.

Solo necesita un JWT válido y firmado del servidor de destino y una [lista de palabras de secretos conocidos](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list) . Luego puede ejecutar el siguiente comando, pasando el JWT y la lista de palabras como argumentos:

```
hashcat -a 0 -m 16500 <jwt> <wordlist>
```

<figure><img src="../../../.gitbook/assets/1 (23).png" alt=""><figcaption></figcaption></figure>

Hashcat firma el encabezado y la carga útil del JWT usando cada secreto en la lista de palabras, luego compara la firma resultante con la original del servidor. Si alguna de las firmas coincide, hashcat genera el secreto identificado en el siguiente formato, junto con varios otros detalles:

```
<jwt>:<identified-secret>
```

Como hashcat se ejecuta localmente en su máquina y no depende del envío de solicitudes al servidor, este proceso es extremadamente rápido, incluso cuando se usa una lista de palabras enorme.

Una vez que haya identificado la clave secreta, puede usarla para generar una firma válida para cualquier encabezado JWT y carga útil que desee. Para obtener detalles sobre cómo volver a firmar un JWT modificado en Burp Suite, consulte [Firma de JWT](https://portswigger.net/web-security/jwt/working-with-jwts-in-burp-suite#signing-jwts) .
