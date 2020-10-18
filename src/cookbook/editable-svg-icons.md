# Sistema de íconos SVG editables

## Ejemplo base

Existen muchas maneras de crear un sistema de íconos SVG, pero un método que aprovecha las capacidades de Vue, es crear íconos _inline_ editables como componentes. Algunas de las ventajas de esta forma de trabajar son:

- Son fáciles de editar en tiempo real
- Son animables
- Puede usar _props_ y valores por defecto, para mantener un tamaño común o moficarlo si es necesario
- Son _inline_, por lo que no se necesitan llamadas HTTP
- Pueden volverse accesibles dinámicamente

Primero, crearemos una carpeta para todos los íconos, y los nombraremos de forma estandarizada para un acceso fácil:

- `components/icons/IconBox.vue`
- `components/icons/IconCalendar.vue`
- `components/icons/IconEnvelope.vue`

Aquí esta un _repo_ de ejemplo, donde puede ver toda la configuración: [https://github.com/sdras/vue-sample-svg-icons/](https://github.com/sdras/vue-sample-svg-icons/)

![Documentation site](https://s3-us-west-2.amazonaws.com/s.cdpn.io/28963/screendocs.jpg 'Docs demo')

Crearemos un componente base para el ícono (`IconBase.vue`) que use un slot:

```html
<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :width="width"
    :height="height"
    viewBox="0 0 18 18"
    :aria-labelledby="iconName"
    role="presentation"
  >
    <title :id="iconName" lang="en">{{ iconName }} icon</title>
    <g :fill="iconColor">
      <slot />
    </g>
  </svg>
</template>
```

Puede usar este ícono base tal como está, lo único que podría tener que cambiar es el `viewBox` dependiendo del `viewBox` de sus íconos. En la base, estamos definiendo como _props_ a `width`, `height`, `iconColor` y al nombre del ícono, de tal manera que puedan ser actualizadas dinámicamente. El nombre será usado para el contenido de `<title>` y de su `id` para accesibilidad.

Nuestro _script_ se verá de la siguiente manera, tendremos algunos valores por defecto para que nuestro ícono sea renderizado de manera consistente, a menos de que lo definamos de otra forma:

```js
export default {
  props: {
    iconName: {
      type: String,
      default: 'box'
    },
    width: {
      type: [Number, String],
      default: 18
    },
    height: {
      type: [Number, String],
      default: 18
    },
    iconColor: {
      type: String,
      default: 'currentColor'
    }
  }
}
```

La propiedad `currentColor`, que es el valor por defecto en el _fill_, hará que el ícono herede el color del texto que lo rodea. También podríamos pasar un color diferente como _prop_ si así lo deseamos.

Lo podemos usar de la siguiente manera, teniendo como único contenido de `IconWrite.vue` los _paths_ dentro del ícono:

```html
<icon-base icon-name="write"><icon-write /></icon-base>
```

Ahora, si quisieramos tener varios tamaños para el ícono, podríamos hacerlo facilmente:

```html
<p>
  <!-- puede pasar valores más pequeños para `width` y `height` como props -->
  <icon-base width="12" height="12" icon-name="write"><icon-write /></icon-base>
  <!-- o puede usar el valor por defecto, el cual es 18 -->
  <icon-base icon-name="write"><icon-write /></icon-base>
  <!-- o hacerlo un poco más grante :) -->
  <icon-base width="30" height="30" icon-name="write"><icon-write /></icon-base>
</p>
```

<img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/28963/Screen%20Shot%202018-01-01%20at%204.51.40%20PM.png" width="450" />

## Íconos animables

Mantener los íconos en los componentes es muy útil cuando se desea animarlos, especialmente en una interacción. Los SVG _inline_ tienen el mayor soporte para la interacción de cualquier método. Aquí hay un ejemplo muy básico de un ícono que está animado al hacer clic:

```html
<template>
  <svg
    @click="startScissors"
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 100 100"
    width="100"
    height="100"
    aria-labelledby="scissors"
    role="presentation"
  >
    <title id="scissors" lang="en">Scissors Animated Icon</title>
    <path id="bk" fill="#fff" d="M0 0h100v100H0z" />
    <g ref="leftscissor">
      <path d="M..." />
      ...
    </g>
    <g ref="rightscissor">
      <path d="M..." />
      ...
    </g>
  </svg>
</template>
```

```js
import { TweenMax, Sine } from 'gsap'

export default {
  methods: {
    startScissors() {
      this.scissorAnim(this.$refs.rightscissor, 30)
      this.scissorAnim(this.$refs.leftscissor, -30)
    },
    scissorAnim(el, rot) {
      TweenMax.to(el, 0.25, {
        rotation: rot,
        repeat: 3,
        yoyo: true,
        svgOrigin: '50 45',
        ease: Sine.easeInOut
      })
    }
  }
}
```

Estamos aplicando `refs` a los grupos de _paths_ que necesitamos mover, y como ambos lados de las tijeras tienen que moverse en tándem, crearemos una función que podemos reutilizar donde pasaremos los `refs`. El uso de _GreenSock_ ayuda a resolver el soporte de animación y los problemas de `transform-origin` en diferentes navegadores.

<p data-height="300" data-theme-id="0" data-slug-hash="dJRpgY" data-default-tab="result" data-user="Vue" data-embed-version="2" data-pen-title="Editable SVG Icon System: Animated icon" class="codepen">See the Pen <a href="https://codepen.io/team/Vue/pen/dJRpgY/">Editable SVG Icon System: Animated icon</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>) on <a href="https://codepen.io">CodePen</a>.</p><script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<p style="margin-top:-30px">¡Logrado de manera secilla! Y fácil de modificar en tiempo real.</p>

Puede ver más ejemplos animados en el _repo_ [aquí](https://github.com/sdras/vue-sample-svg-icons/)

## Notas adicionales

Los diseñadores pueden cambiar de opinión. Los requisitos del producto cambian. Mantener la lógica de todo el sistema de íconos en un componente base significa que puede actualizar rápidamente todos sus íconos y hacer que se propague a través de todo el sistema. Incluso con el uso de un _icon loader_, algunas situaciones requieren que vuelva a rehacer o editar cada SVG para realizar cambios globales. Este método puede ahorrarle tiempo y esfuerzo.

## Cuándo evitar este patrón

Este tipo de sistema de íconos SVG es realmente útil cuando tiene varios íconos que se utilizan de diferentes maneras en todo su sitio. Si está repitiendo el mismo ícono muchas veces en una página (por ejemplo, una tabla gigante con un ícono de eliminar en cada fila), podría tener más sentido que todos los _sprites_ se compilen en una hoja aparte, y utilicen las etiquetas `<use>` para cargarlos.

## Patrones alternativos

Otras herramientas para ayudar a manejar iconos SVG son:

- [svg-sprite-loader](https://github.com/kisenka/svg-sprite-loader)
- [svgo-loader](https://github.com/rpominov/svgo-loader)

Estas herramientas agrupan SVGs en tiempo de compilación, pero hacen que sea un poco más difícil de editar durante tiempo de ejecución, porque las etiquetas `<use>` pueden tener comportamientos extraños en diferentes navegadores cuando se hace algo más complejo. También dejan dos propiedades anidadas de `viewBox` y por lo tanto dos sistemas de coordenadas. Esto hace que la implementación sea un poco más compleja.
