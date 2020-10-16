# Introducción

## El Libro de Recetas vs la Guía

¿En qué se diferencia el libro de recetas de la guía? ¿Por qué es esto necesario?

- **Mayor enfoque**: En la guía, esencialmente estamos contando una historia. Cada sección se asume y se construye sobre el conocimiento de cada sección anterior. En el libro de recetas, cada receta puede y debería valer por sí sola. Esto significa que las recetas pueden centrarse en un aspecto específico de Vue, en lugar de tener que dar una visión general.

- **Mayor profundidad**: Para evitar que la guía sea demasiado larga, tratamos de incluir solo los ejemplos más simples posibles, y así, ayudarlo a entender cada característica. Entonces seguimos adelante. En el libro de recetas, podemos incluir ejemplos más complejos, combinando características en formas interesantes. Cada receta también puede ser tan larga y detallada, como sea necesario, para explorar completamente su nicho.

- **Enseñando JavaScript**: En la guía, asumimos al menos familiaridad intermedia con JavaScript ES5. Por ejemplo, no explicaremos cómo funciona `Array.prototype.filter` en una propiedad computada que filtra una lista. Sin embargo, en el libro de recetas, se pueden explorar y explicar características esenciales de JavaScript (incluido ES6/2015+) en el contexto de cómo nos ayudan a construir mejores aplicaciones de Vue.

- **Explorando el ecosistema**: Para características avanzadas, asumimos algún conocimiento del ecosistema. Por ejemplo, si desea usar componentes de un solo archivo en Webpack, no explicamos cómo configurar las partes que no son de Vue de la configuración de Webpack. En el libro de recetas, tenemos el espacio para explorar estas librerías del ecosistema en mayor profundidad, al menos en la medida en que sea universalmente útil para los desarrolladores de Vue.

::: tip
Con todas estas diferencias, por favor tome en cuenta que el libro de recetas _no es_ un manual paso a paso. Para la mayoría del contenido, se espera que tenga una comprensión básica de conceptos como HTML, CSS, Javascript, npm/yarn, etcétera.
:::

## Contribuciones del libro de recetas

### Lo que estamos buscando

El libro de recetas proporciona ejemplos típicos de la vida real o interesantes para que los desarrolladores puedan usar como modelo, y también explica progresivamente detalles más complejos. Nuestro objetivo es ir más allá de un simple ejemplo introductorio, y demostrar los conceptos que son más aplicables, así como algunas advertencias al enfoque utilizado.

Si está interesado en contribuir, inicie la colaboración reportando un _issue_ con su idea y con la etiqueta **cookbook idea**, para que podamos guiarlo hacia la creación de un _pull request_ exitoso. Después de que su idea haya sido aprobada, por favor siga tanto como sea posible la plantilla que se muestra a continuación. Algunas secciones son requeridas, y otras son opcionales. Seguir el orden numérico se sugiere fuertemente, pero no es obligatorio.

Las recetas generalmente deben:

- Resolver un problema específico y común.
- Comenzar con el ejemplo más simple posible.
- Introducir las complejidades una a la vez.
- Enlazar a otros documentos, en lugar de re-explicar conceptos.
- Describir el problema, en lugar de asumir familiaridad.
- Explicar el proceso, en lugar de solo mostrar el resultado.
- Explicar las ventajas y desventajas de su estrategia, incluso, cuándo es apropiado y cuándo no.
- Mencione soluciones alternativas si son relevantes, pero deje las exploraciones en profundidad en una receta separada.

Le solicitamos que siga la siguiente plantilla. Sin embargo, entendemos que hay ocasiones en las que es posible que deba desviarse por claridad o flujo. De cualquier manera, todas las recetas deben discutir en algún momento el matiz de la elección hecha usando este patrón, preferiblemente en la sección de patrones alternativos.

### Ejemplo base <Badge text="requerido" type="error" />

1.  Articule el problema en una o dos oraciones.
2.  Explique la solución más simple posible en una o dos oraciones.
3.  Muestre un ejemplo de código pequeño.
4.  Explique lo que esto logra en una oración.

### Detalles sobre el valor <Badge text="requerido" type="error" />

1.  Responda a las preguntas comunes que uno pueda tener mientras mira el ejemplo (las citas de bloque son geniales para esto).
2.  Muestre ejemplos de errores comunes y cómo pueden evitarse.
3.  Muestre ejemplos de código simples, de patrones buenos y malos.
4.  Discuta por qué esto puede ser un patrón convincente. No se requieren enlaces de referencia, pero se recomienda.

### Ejemplo del mundo real <Badge text="requerido" type="error" />

Demuestre el código que podría impulsar un caso de uso común o interesante, ya sea:

1.  Una recorrida a través de unos pocos ejemplos de configuración, o
2.  Incorporar un ejemplo de codepen/jsfiddle

Si elige hacer lo último, aún debe hablar sobre qué es y qué hace.

### Contexto adicional <Badge text="opcional" />

Es extremadamente útil escribir un poco sobre este patrón, en qué otro lugar se aplicaría, por qué funciona bien y ejecutar un poco de código a medida que lo hace o dar a las personas más material de lectura aquí.

### Cuándo evitar este patrón <Badge text="opcional" />

Esta sección no es requerida, pero altamente recomendable. No tiene sentido escribirlo para algo muy simple, como alternar clases en función del cambio de estado, pero para patrones más avanzados como los mixins es vital. La respuesta a la mayoría de las preguntas sobre el desarrollo es [“¡Depende!”](https://codepen.io/rachsmith/pen/YweZbG), esta sección abarca eso. Aquí, analizaremos con honestidad cuándo es útil el patrón y cuándo debería evitarse, o cuando algo tiene más sentido.

### Patrones alternativos <Badge text="requerido con la sección de 'evitar este patrón'" type="warning" />

Esta sección es obligatoria cuando ha proporcionado la sección anterior sobre prevención. Es importante explorar otros métodos para que las personas, que dicen que algo es un antipatrón en ciertas situaciones, no se queden preguntándose. Al hacerlo, considere que la web es una gran carpa y que muchas personas tienen diferentes estructuras de base de código y están resolviendo diferentes objetivos. ¿La aplicación es grande o pequeña? ¿Están integrando Vue en un proyecto existente o están construyendo desde cero? ¿Sus usuarios solo están tratando de lograr un solo objetivo o muchos objetivos? ¿Hay muchos datos asíncronos? Todas estas preocupaciones impactarán en implementaciones alternativas. Una buen libro de recetas da a los desarrolladores este contexto.

## Gracias

Se necesita tiempo para contribuir a la documentación, y si invierte tiempo para enviar un _PR_ a esta sección, lo estará haciendo con nuestro agradecimiento.
