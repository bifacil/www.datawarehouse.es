---
UniqueId: ydnEMkcNWC
Title: "Consejo de diseño #168. Que hay en un nombre"
Url: 2014/que-hay-en-un-nombre.html
Section: "Artículos"
Date: 2014-06-21T00:00:00.0000000+02:00
Description: "Aunque parezca una cosa sin importancia, los nombres son importantes. Nombrar correctamente tablas y columnas es particularmente importante para los usuarios ad hoc del sistema DW/BI para que puedan encontrar lo que buscan. Los nombres de los objetos deben estar orientados a los usuarios de negocios, no al personal técnico."
Author: Joy Mundy

---
Aunque parezca una cosa sin importancia, los nombres son importantes. Nombrar correctamente tablas y columnas es particularmente importante para los usuarios ad hoc del sistema DW/BI para que puedan encontrar lo que buscan. Los nombres de los objetos deben estar orientados a los usuarios de negocios, no al personal técnico.

Esfuérzate tanto como sea posible en no cambiar los nombres del sistema DW/BI en la capa semántica ni en los informes. Más complicado todavía: Se debe desincentivar que los usuarios cambien los nombres de los objetos una vez han seleccionado la información. Por lo general, no podemos impedir que lo hagan, pero nombres atractivos y bien pensados reducirán la tentación de cambio.

Aquí están mis 10 mejores recomendaciones para nombrar objetos en el almacén de datos de tu sistema de inteligencia empresarial (BI).

### 1. Sigue la convención de nomenclatura.

Si no la tienes, crea (y documenta) una convención de nomenclatura que siga las reglas de este consejo de diseño. Si tu organización ya tiene convención para la nomenclatura, te estarás enfrentando a un problema: la mayoría de las convenciones de nomenclatura existentes han sido desarrolladas por personal técnico. Pero los nombres en el entorno DW/BI deben estar orientadas a los usuarios de negocios.  Se convierten en nombres de columnas y filas en los análisis ad hoc y en los informes predefinidos. Volveremos a este tema más adelante.

### 2. Cada objeto tiene un nombre.

No perpetuemos la confusión en la definición de datos ya existente en nuestras organizaciones. No es correcto decir que el equipo de ventas nombra a una columna Geografía y el equipo de marketing nombra la misma columna como Región. Si no puedes llegar a un  acuerdo global en la organización para nombrar objetos, debes solicitar la ayuda de tu responsable de negocios.

### 3. Los nombres de los objetos son descriptivos.

Los usuarios no necesitar estar 20 años en tu organización para poder descifrar el significado de un nombre. Esta regla evita muchas tonterías, como RBVLSPCD (¡tenemos más de 8 carácteres para trabajar!). También prohíbe columnas como NOMBRE, que no es descriptivo fuera del contexto de la tabla que estás examinando.

### 4. No se recomiendan abreviaciones y acrónimos.

Las abreviaciones y acrónimos son endémicas en el mundo corporativo, y en el mundo no corporativo es incluso peor. Mucha información se puede codificar en un acrónimo pero para los novatos supone una enorme carga. El enfoque más efectivo es mantener una lista de abreviaciones aprobadas, e intentar no ampliar sin una buena razón. Deberías incluso documentar esa razón en la lista.

Algunos ejemplos incluirían:

<mark>IMAGEN</mark>

Para la mayoría de organizaciones, una lista razonable tiene docenas de abreviaciones aceptadas, no cientos.

### 5. Los nombres de los objetos son atractivos.

Recuerda que los nombres de los objetos se convierten en encabezados en informes y análisis. Aunque el atractivo está en el ojo del que lee, para mí son molestas todas las mayúsculas. Los nombres de objetos deben contener una pista visual sobre dónde terminan las palabras.

- Espacios: [Columna Nombre]
- Camel case: Columna Nombre
- Destacar: Columna\_Nombre o COLUMNA\_NOMBRE

Recomiendo utilizar espacios. Quedan mejor al incluirlos en los informes. Y me río frente al argumento de que los desarrolladores tienen que mecanografiar corchetes cuando escriben el nombre de la columna. Estoy seguro de que cualquier desarrollador que actualmente teclee SQL puede descifrar donde están colocados los paréntesis, y pueden desarrollar los músculos de los dedos necesarios.

### 6. Los nombres de los objetos son únicos.

Esta regla es el corolario a la regla de que cada objeto tiene un nombre. Si dos objetos son diferentes, deben tener nombres diferentes. Esta regla prohíbe nombres de columna como [Ciudad]. Un nombre mejor es [ ciudad del cliente]. Esta regla es especialmente importante para el uso ad hoc del sistema DW/BI. Aunque el contexto de [Ciudad] es obvio durante el proceso analítico, una vez que el contexto es guardado y compartido, se pierde. ¿Buscamos la ciudad del cliente o la del almacén? ¿la dirección postal o la dirección de envío? Aunque no podemos prevenir que los usuarios cambien el nombre de los objetos una vez exporten a Excel, no queremos forzarlos a hacerlo para así estar más claro.

### 7. Nombres de objetos no demasiado largos.

Esta regla entra en conflicto con las reglas 3, 4 y 6. Si tenemos nombres de objetos únicos, sin abreviar, descriptivos es probable que algunos nombres de columnas sean muy largos. Consideremos [Código de Salida de la Carta Obligatoria del Segundo Pagador] o [Proveedor de la Categoría de Referencia]. Estas descripciones de columnas son razonables en el negocio de los seguros, ¿qué pasa cuando el usuario o el que realiza el informe utiliza esta columna para realizar un informe o análisis? El nombre no quedará nada bien y el encabezamiento de la fila estará demasiado lleno. Se tendría que reducir hasta un tamaño de letra tan minúsculo que nadie podría leerlo. Entonces el usuario renombraría la columna, violando nuestra regla clave de que cada objeto tiene un nombre. Yo intento limitar los nombres de las columnas a 30  caracteres, aunque a veces llego a 35. Para poder cumplir este objetivo, necesito registrar más abreviaciones o acrónimos de los que me gustaría.

### 8. Considera anteponer a las columnas de nombre, una tabla abreviada del nombre.

Odio hacer esta recomendación, porque viola varias de mis reglas previas sobre abreviaciones y nombres cortos. Pero yo mismo me encuentro siguiendo esta práctica con frecuencia para garantizar la consistencia y unicidad.

### 9. Cambia los nombres en la capa de visualización si es necesario.

Siempre hemos recomendado una *capa de vistas* en el sistema DW/BI que se coloquen sobre las tablas físicas.

El objetivo principal de la capa de visualización es establecer una capa que aísle las aplicaciones BI de la base de datos física, proporcionando un poco de flexibilidad para facilitar el camino de migrar a un sistema ya en producción. Adicionalmente, la capa de visualización puede proporcionar también un lugar para poner los nombres orientados al negocio en la base de datos.

Nuestra primera recomendación es siempre nombrar a las tablas y columnas física con nombres orientados al negocio. Si esto falla, utiliza la capa de visualización. No nos gusta cambiar los nombres en las herramientas BI por diversas razones:

1. La mayoría de organizaciones tienes diferentes herramientas BI; los nombres deben ser  coherentes para utilizarlos en la totalidad de las herramientas BI.
2. Cuanta más lógica de negocios apliques en la herramienta BI, mayor será el reto de migrar a una herramienta diferente.
3. Si los nombres están solamente en la herramienta BI, existe una barrera comunicativa entre usuarios, el equipo de atención al cliente y la gente de mantenimiento de la base de datos.

### 10. ¡Se constante!

La necia coherencia es el duende de las mentes pequeñas. La coherencia en la nomenclatura es sumamente valiosa para sus usuarios.

Artículo original: [http://www.kimballgroup.com/2014/07/design-tip-168-whats-name/][1]





[1]: http://www.kimballgroup.com/2014/07/design-tip-168-whats-name/