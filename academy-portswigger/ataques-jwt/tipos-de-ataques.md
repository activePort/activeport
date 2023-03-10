# Tipos de ataques

Los ataques JWT involucran a un usuario que envía JWT modificados al servidor para lograr un objetivo malicioso. Por lo general, este objetivo es eludir la autenticación y [los controles de acceso ](https://portswigger.net/web-security/access-control)haciéndose pasar por otro usuario que ya se ha autenticado.



## <mark style="color:orange;">Impacto de los ataques JWT</mark>

El impacto de los ataques JWT suele ser grave. Si un atacante puede crear sus propios tokens válidos con valores arbitrarios, puede escalar sus propios privilegios o hacerse pasar por otros usuarios, tomando el control total de sus cuentas.



## <mark style="color:orange;">¿Como surgen las vulnerabilidades a los ataques JWT?</mark>

Las vulnerabilidades de JWT suelen surgir debido a un manejo defectuoso de JWT dentro de la propia aplicación. Las [diversas especificaciones](https://portswigger.net/web-security/jwt#jwt-vs-jws-vs-jwe) relacionadas con los JWT tienen un diseño relativamente flexible, lo que permite a los desarrolladores de sitios web decidir muchos detalles de implementación por sí mismos. Esto puede resultar en que introduzcan vulnerabilidades accidentalmente incluso cuando se usan bibliotecas reforzadas.

Estas fallas de implementación generalmente significan que la firma del JWT no se verifica correctamente. Esto permite que un atacante manipule los valores pasados a la aplicación a través de la carga útil del token.Incluso si la firma se verifica sólidamente, si realmente se puede confiar en ella depende en gran medida de que la clave secreta del servidor permanezca en secreto. Si esta clave se filtra de alguna manera, o se puede adivinar o utilizar la fuerza bruta, un atacante puede generar una firma válida para cualquier token arbitrario, comprometiendo todo el mecanismo.
