# Prevencion

## <mark style="color:orange;">Como prevenir los atauqes JWT</mark>

Puede proteger sus propios sitios web contra muchos de los ataques que hemos cubierto tomando las siguientes medidas de alto nivel:

* Utilice una biblioteca actualizada para manejar JWT y asegúrese de que sus desarrolladores entiendan completamente cómo funciona, junto con las implicaciones de seguridad. Las bibliotecas modernas hacen que sea más difícil implementarlas de forma insegura sin darse cuenta, pero esto no es infalible debido a la flexibilidad inherente de las especificaciones relacionadas.
* Asegúrese de realizar una verificación de firma sólida en cualquier JWT que reciba y tenga en cuenta los casos extremos, como los JWT firmados con algoritmos inesperados.
* Hacer cumplir una lista blanca estricta de hosts permitidos para el `jku`encabezamiento.
* Asegúrese de que no es vulnerable al [recorrido de ruta ](https://portswigger.net/web-security/file-path-traversal)o inyección de SQL a través de la `kid`parámetro de encabezado.

## <mark style="color:orange;">Mejores practicas adicionales para el manejo de JWT</mark>

Aunque no es estrictamente necesario para evitar la introducción de vulnerabilidades, recomendamos seguir las siguientes prácticas recomendadas al usar JWT en sus aplicaciones:

* Establezca siempre una fecha de vencimiento para cualquier token que emita.
* Evite enviar tokens en parámetros de URL siempre que sea posible.
* Incluir la `aud`(audiencia) reclamación (o similar) para especificar el destinatario previsto del token. Esto evita que se utilice en diferentes sitios web.
* Permita que el servidor emisor revoque tokens (al cerrar la sesión, por ejemplo).
