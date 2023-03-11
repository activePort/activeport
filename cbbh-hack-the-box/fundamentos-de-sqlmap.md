---
cover: ../.gitbook/assets/sqlmap_bg2.jpg
coverY: 0
---

# Fundamentos de SQLMap

La mayoría de las aplicaciones web en estos días están conectadas a una base de datos en el backend que almacena varios tipos de datos que la página web necesita mostrar, desde la información del usuario hasta el contenido del front-end.

Es muy común que dichas aplicaciones web implementen incorrectamente llamadas a la base de datos, lo que hace posible que un atacante manipule las solicitudes HTTP enviadas al servidor para engañar a la base de datos para que muestre más datos de los que pretendían los desarrolladores. Este tipo de ataque se denomina inyección SQL (SQLi).

SQLMap es una herramienta automatizada que se especializa en automatizar el descubrimiento y la explotación de inyección SQL, lo que hace que sea muy trivial para los pentesters descubrir y explotar vulnerabilidades de inyección SQL.

En el `SQLMap Essentials`aprenderá los conceptos básicos del uso de SQLMap para descubrir varios tipos de vulnerabilidades de inyección SQL, hasta la enumeración avanzada de bases de datos y la recuperación de datos interesantes.



En este módulo, cubriremos:

* Descripción general e instalación de SQLMap.
* Diferentes tipos de ataques de inyección SQL admitidos por SQLMap y dónde usar cada uno.
* Comprender los diversos resultados de SQLMap, para guiar adecuadamente sus ataques.
* Atacar partes específicas de una aplicación web con el uso de solicitudes HTTP.
* Tratar con varios tipos de errores que podemos enfrentar al usar SQLMap.
* Usar varias opciones de SQLMap para ajustar los ataques a nuestras necesidades específicas.
* Enumerar bases de datos completas y extraer el contenido de sus tablas, columnas y filas.
* Enumeración avanzada de bases de datos para encontrar datos específicos.
* Encontrar nombres de usuario y contraseñas dentro de las bases de datos y usar SQLMap para descifrarlos.
* Omitir varios tipos de protecciones que pueden existir para proteger la aplicación web contra SQLMap.
* Uso de SQLMap para leer y escribir archivos en el servidor remoto.
* Usar SQLMap para ejecutar comandos en el servidor remoto y tomar control completo sobre él.



