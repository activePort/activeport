# Descripcion de salida

Al final de la sección anterior, la salida de sqlmap nos mostró mucha información durante su exploración. Estos datos suelen ser cruciales para comprender, ya que nos guían a través del proceso automatizado de inyección de SQL. Esto nos muestra exactamente qué tipo de vulnerabilidades está explotando SQLMap, lo que nos ayuda a reportar qué tipo de inyección tiene la aplicación web. Esto también puede resultar útil si queremos explotar manualmente la aplicación web una vez que SQLMap determina el tipo de inyección y el parámetro vulnerable.



## <mark style="color:orange;">Descripcion de los mensajes de registro</mark>

Los siguientes son algunos de los mensajes más comunes que se suelen encontrar durante un análisis de SQLMap, junto con un ejemplo de cada uno del ejercicio anterior y su descripción.



### <mark style="color:orange;">El contenido de la URL es estable</mark>

<mark style="color:orange;">Log Message :</mark> "target URL content is stable"

Esto significa que no hay cambios importantes entre las respuestas en caso de solicitudes idénticas continuas. Esto es importante desde el punto de vista de la automatización ya que, en caso de respuestas estables, es más fácil detectar las diferencias causadas por los posibles intentos de <mark style="color:orange;">`SQLi`</mark>. Si bien la estabilidad es importante, SQLMap tiene mecanismos avanzados para eliminar automáticamente el "ruido" potencial que podría provenir de objetivos potencialmente inestables.



## <mark style="color:orange;">El parametro parece ser dinamico</mark>

<mark style="color:orange;">Log message :</mark> "GET parameter 'id' appears to be dynamic"

Siempre se desea que el parámetro probado sea "dinámico", ya que es una señal de que cualquier cambio realizado en su valor daría como resultado un cambio en la respuesta; por lo tanto, el parámetro puede estar vinculado a una base de datos. En caso de que la salida sea "estática" y no cambie, podría ser un indicador de que el objetivo no procesa el valor del parámetro probado, al menos en el contexto actual.



## <mark style="color:orange;">**El parámetro puede ser inyectable**</mark>

<mark style="color:orange;">Log message</mark>  :  "heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')"

Como se discutió anteriormente, los errores de DBMS son una buena indicación del potencial SQLi. En este caso, hubo un error de MySQL cuando SQLMap envió un valor intencionalmente no válido (p. ej. `?id=1",)..).))'`), lo que indica que el parámetro probado podría ser SQLi inyectable y que el objetivo podría ser MySQL. Cabe señalar que esto no es una prueba de SQLi, sino solo una indicación de que el mecanismo de detección debe probarse en la ejecución posterior.\


## <mark style="color:orange;">**El parámetro puede ser vulnerable a ataques XSS**</mark>

<mark style="color:orange;">Log message</mark> <mark style="color:orange;"></mark><mark style="color:orange;">****</mark>** :** "heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks"

Si bien no es su propósito principal, SQLMap también ejecuta una prueba heurística rápida para detectar la presencia de una vulnerabilidad XSS. En las pruebas a gran escala, donde se prueban muchos parámetros con SQLMap, es bueno tener este tipo de verificaciones heurísticas rápidas, especialmente si no se encuentran vulnerabilidades de SQLi.



## <mark style="color:orange;">**DBMS back-end es '...'**</mark>

<mark style="color:orange;">Log message :</mark>  "it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? \[Y/n]"

En una ejecución normal, SQLMap prueba todos los DBMS admitidos. En caso de que haya una indicación clara de que el objetivo está utilizando el DBMS específico, podemos reducir las cargas útiles solo a ese DBMS específico.

<mark style="color:orange;">****</mark>

## <mark style="color:orange;">**Valores de nivel/riesgo**</mark>

<mark style="color:orange;">Log message :</mark> "for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? \[Y/n]”

Si hay una indicación clara de que el destino utiliza el DBMS específico, también es posible extender las pruebas para ese mismo DBMS específico más allá de las pruebas regulares. Básicamente, esto significa ejecutar todas las cargas útiles de inyección de SQL para ese DBMS específico, mientras que si no se detectara ningún DBMS, solo se probarían las cargas útiles principales.



## <mark style="color:orange;">Valores reflectivos encontrados</mark>

<mark style="color:orange;">Log messsage</mark> : "reflective value(s) found and filtering out"

Solo una advertencia de que partes de las cargas útiles utilizadas se encuentran en la respuesta. Este comportamiento podría causar problemas a las herramientas de automatización, ya que representa la basura. Sin embargo, SQLMap tiene mecanismos de filtrado para eliminar dicha basura antes de comparar el contenido de la página original.



## <mark style="color:orange;">El parametro parece ser inyectable</mark>

<mark style="color:orange;">Log message</mark> : "GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="luther")"

Este mensaje indica que el parámetro parece ser inyectable, aunque todavía existe la posibilidad de que sea un resultado falso positivo. En el caso de tipos de SQLi ciegos y similares basados en booleanos (p. ej., ciegos basados en el tiempo), donde existe una alta probabilidad de falsos positivos, al final de la ejecución, SQLMap realiza pruebas exhaustivas que consisten en verificaciones lógicas simples para eliminar de hallazgos falsos positivos.

Además, `with --string="luther"`indica que SQLMap reconoció y usó la apariencia de un valor de cadena constante `luther`en la respuesta para distinguir `TRUE`de `FALSE`respuestas.Este es un hallazgo importante porque en tales casos, no es necesario el uso de mecanismos internos avanzados, como la eliminación de dinamicidad/reflexión o la comparación difusa de respuestas, que no pueden considerarse como falsos positivos.



## <mark style="color:orange;">**Modelo estadístico de comparación basado en el tiempo**</mark>

<mark style="color:orange;">Log message</mark> : "time-based comparison requires a larger statistical model, please wait........... (done)"

SQLMap utiliza un modelo estadístico para el reconocimiento de respuestas objetivo regulares y (deliberadamente) retrasadas. Para que este modelo funcione, existe el requisito de recopilar una cantidad suficiente de tiempos de respuesta regulares. De esta manera, SQLMap puede distinguir estadísticamente entre el retraso deliberado incluso en entornos de red de alta latencia.



## <mark style="color:orange;">**Ampliación de las pruebas de la técnica de inyección de consultas UNION**</mark>

<mark style="color:orange;">Log Messsage</mark> : "automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found”

Las comprobaciones <mark style="color:orange;">`SQLi`</mark> de consulta <mark style="color:orange;">`UNION`</mark> requieren considerablemente más solicitudes para el reconocimiento exitoso de la carga útil que otros tipos de <mark style="color:orange;">`SQLi`</mark>. Para reducir el tiempo de prueba por parámetro, especialmente si el objetivo no parece inyectable, la cantidad de solicitudes se limita a un valor constante (es decir, 10) para este tipo de verificación. Sin embargo, si existe una buena posibilidad de que el destino sea vulnerable, especialmente cuando se encuentra otra técnica <mark style="color:orange;">`SQLi`</mark> (potencial), <mark style="color:orange;">`SQLMap`</mark> amplía el número predeterminado de solicitudes para <mark style="color:orange;">`UNION`</mark> query SQLi, debido a una mayor expectativa de éxito.





## <mark style="color:orange;">**La técnica parece ser utilizable**</mark>

<mark style="color:orange;">Log message</mark> : "ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test"

Como verificación heurística para el tipo SQLi de consulta UNION, antes de la UNION se envían payloads, una técnica conocida como ORDER BY se comprueba la usabilidad. En caso de que sea utilizable, SQLMap puede reconocer rápidamente el número correcto de UNION columnas realizando el enfoque de búsqueda binaria.

{% hint style="info" %}
Tenga en cuenta que esto depende de la tabla afectada en la consulta vulnerable.
{% endhint %}



## <mark style="color:orange;">El parametro es vulnerable</mark>

<mark style="color:orange;">Log message</mark> : "GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? \[y/N]"

Este es uno de los mensajes más importantes de SQLMap, ya que significa que se descubrió que el parámetro es vulnerable a las inyecciones de SQL. En los casos habituales, es posible que el usuario solo desee encontrar al menos un punto de inyección (es decir, un parámetro) utilizable contra el objetivo. Sin embargo, si estamos ejecutando una prueba exhaustiva en la aplicación web y queremos informar todas las vulnerabilidades potenciales, podemos continuar buscando todos los parámetros vulnerables.



## <mark style="color:orange;">**Sqlmap identificó puntos de inyección**</mark>

<mark style="color:orange;">Log message</mark> : "sqlmap identified the following injection point(s) with a total of 46 HTTP(s) requests:”

A continuación se muestra una lista de todos los puntos de inyección con tipo, título y cargas útiles, que representa la prueba final de detección y explotación exitosas de las vulnerabilidades de SQLi encontradas. Debe tenerse en cuenta que SQLMap enumera solo aquellos hallazgos que son comprobablemente explotables (es decir, utilizables).



## <mark style="color:orange;">**Datos registrados en archivos de texto**</mark>

<mark style="color:orange;">Log message</mark> : "fetched data logged to text files under '/home/user/.sqlmap/output/www.example.com'"

Esto indica la ubicación del sistema de archivos local que se usa para almacenar todos los registros, sesiones y datos de salida para un destino específico; en este caso, <mark style="color:orange;">`www.example.com`</mark>. Después de una ejecución inicial de este tipo, donde el punto de inyección se detecta con éxito, todos los detalles para futuras ejecuciones se almacenan dentro de los archivos de sesión del mismo directorio. Esto significa que <mark style="color:orange;">`SQLMap`</mark> intenta reducir las solicitudes de destino requeridas tanto como sea posible, según los datos de los archivos de sesión.
