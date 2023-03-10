# Firma defectuosa

## <mark style="color:orange;">EXPLOTACION DE VERIFICACION DE FIRMA JWT DEFECTUOSA</mark>

Por diseño, los servidores no suelen almacenar ninguna información sobre los JWT que emiten. En cambio, cada token es una entidad completamente autónoma. Esto tiene varias ventajas, pero también presenta un problema fundamental: el servidor en realidad no sabe nada sobre el contenido original del token, ni siquiera cuál era la firma original. Por lo tanto, si el servidor no verifica la firma correctamente, no hay nada que impida que un atacante realice cambios arbitrarios en el resto del token.

Por ejemplo, considere un JWT que contenga las siguientes afirmaciones:

```json
{
    "username": "carlos",
    "isAdmin": false
}
```

Si el servidor identifica la sesión basándose en este `username`,la modificación de su valor podría permitir que un atacante se haga pasar por otros usuarios registrados. Del mismo modo, si el valor `isAdmin` se utiliza para el control de acceso, esto podría proporcionar un vector simple para la escalada de privilegios.



## <mark style="color:orange;">Aceptar firmas arbitrarias</mark>

Las bibliotecas JWT suelen proporcionar un método para verificar tokens y otro que simplemente los decodifica. Por ejemplo, la biblioteca Node.js jsonwebtoken tiene verify() y decode(). Ocasionalmente, los desarrolladores confunden estos dos métodos y solo pasan tokens entrantes al decode() método. Esto significa efectivamente que la aplicación no verifica la firma en absoluto.
