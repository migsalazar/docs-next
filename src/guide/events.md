# Manejo de eventos

## Escuchar Eventos

Podemos usar la directiva `v-on`, que normalmente acortamos con el símbolo `@`, para escuchar eventos del DOM y ejecutar algo de Javascript cuando son activados. El uso sería `v-on:click="methodName"` o con la forma corta, `@click="methodName"`.

Por ejemplo:

```html
<div id="basic-event">
  <button @click="counter += 1">Agregar 1</button>
  <p>Se ha hecho click {{ counter }} veces en el botón de arriba.</p>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      counter: 1
    }
  }
}).mount('#basic-event')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="xxGadPZ" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Event handling: basic">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/xxGadPZ">
  Event handling: basic</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Métodos de manejo de eventos

Sin embargo, la mayoría de las veces la lógica para el manejo de eventos será más compleja, por lo que mantener el JavaScript en el valor del atributo `v-on` no es factible. Es por eso que `v-on` también puede aceptar el nombre del método que se necesite llamar.`

Por ejemplo:

```html
<div id="event-with-method">
  <!-- `greet` es el nombre del método definido a continuación -->
  <button @click="greet">Saludar</button>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      name: 'Vue.js'
    }
  },
  methods: {
    greet(event) {
      // `this` dentro de los métodos apunta a la instancia activa actual
      alert('Hello ' + this.name + '!')
      // `event` es el evento nativo del DOM
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
}).mount('#event-with-method')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="jOPvmaX" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Event handling: with a method">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/jOPvmaX">
  Event handling: with a method</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Methods in Inline Handlers

Instead of binding directly to a method name, we can also use methods in an inline JavaScript statement:
En lugar de enlazar directamente a un nombre de método, también podemos usar métodos con una instrucción de JavaScript _inline_:

```html
<div id="inline-handler">
  <button @click="say('hi')">Say hi</button>
  <button @click="say('what')">Say what</button>
</div>
```

```js
Vue.createApp({
  methods: {
    say(message) {
      alert(message)
    }
  }
}).mount('#inline-handler')
```

Result:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="WNvgjda" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Event handling: with an inline handler">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/WNvgjda">
  Event handling: with an inline handler</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

A veces también necesitamos acceder al evento DOM original en un en una instrucción _inline_. Este se puede pasar a un método usando la variable `$event`:

```html
<button @click="warn('Form cannot be submitted yet.', $event)">
  Enviar
</button>
```

```js
// ...
methods: {
  warn(message, event) {
    // now we have access to the native event
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```

## Multiple Event Handlers
Muliples Manejadores de Eventos

También puedes tener multiples métodos en un manejador de eventos separando con el operador coma, de la siguiente forma: 

```html
<!-- amobos métodos one() y two() se ejecurán en el evento click del botón -->
<button @click="one($event), two($event)">
  Enviar
</button>
```

```js
// ...
methods: {
  one(event) {
    // lógica del primer handler...
  },
  two(event) {
    // Lógicadel segundo handler...
  }
}
```

## Modificadores de eventos

Es una muy común llamar `event.preventDefault ()` o `event.stopPropagation ()` dentro de los manejadores de eventos. Aunque podemos hacer esto fácilmente dentro de métodos, sería mejor si los métodos pudieran ser puramente lógica de datos en lugar de tener que lidiar con detalles de los eventos del DOM.
`
Para abordar este problema, Vue provee **modificadores de eventos** para `v-on`. Recuerda que los modificadores son prefijos de la directiva denotados por un punto.

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- la propagación del evento click se detendrá -->
<a @click.stop="doThis"></a>

<!-- el evento submit no recargará la página -->
<form @submit.prevent="onSubmit"></form>

<!-- los modificadores pueden ser encadenados -->
<a @click.stop.prevent="doThat"></a>

<!-- solo el modificador -->
<form @submit.prevent></form>

<!-- usar el modo 'capture' al agregar el listener del vento -->
<!-- es decir, un evento que dirigido a un elemento interno, es manejado aquí antes de ser manejado por ese elemento -->
<div @click.capture="doThis">...</div>

<!-- solo lanza el manejador si event.target es el elemento mismo -->
<!-- es deicr, no de un elemento hijo -->
<div @click.self="doThat">...</div>
```

::: tip
El orden es importante cuando se utilizan modificadores porque el código relevante se genera en el mismo orden. Por lo tanto, usar `@click.prevent.self` evitará **todos los clics** mientras que `@click.self.prevent` solo evitará los clics en el elemento en sí.
:::

```html
<!-- el evento click será lanzado máximo una vez -->
<a @click.once="doThis"></a>
```

A diferencia de otros modificadores, que son exclusivos para eventos DOM nativos, el modificador `.once` también se puede usar en [component events](component-custom-events.html). Si aún no ha leído sobre componentes, no se preocupe por esto por ahora.



Vue también ofrece el modificador , correspondiente a [la opción `passive` de `addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters).

```html
<!-- el comportamiento por defecto del scroll ocurrirá -->
<!-- inmediatamente, en lugar de esperar a que se complete `onScroll` -->
<!-- en caso de que contenga `event.preventDefault()`                -->
<div @scroll.passive="onScroll">...</div>
```

El modificador `.passive` es esecialmente útil para mejorar el rendimiento en dispositivos móbiles.

::: tip
No use `.passive` y `.prevent` juntos, porque `.prevent` será ignorado y probablemente su navegador mostrará una advertencia. Recuerde, que `.passive` comunica le al navegador que _no se desea_ evitar el comportamiento predeterminado del evento.
:::

## Modificadores de teclas

Al escuchar eventos del teclado, a menudo necesitamos buscar teclas específicas. Vue permite agregar modificadores de tecla para `v-on` o `@` al escuchar eventos de tecla:

```html
<!-- solo llama a `vm.submit()` cuando la tecla es `Enter` -->
<input @keyup.enter="submit" />
```

Puede usar directamente cualquier clave de tecla válido expuesta a través de [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) como modificadores convirtiéndolos a _kebab-case_.

```html
<input @keyup.page-down="onPageDown" />
```

En el ejemplo anterior, solo se llamará al manejador si `$event.key` es igual a `'PageDown'`.

### Alias de Teclas

Vue provee alias para las teclas más utilizadas:

- `.enter`
- `.tab`
- `.delete` (captura ambas teclas "Delete" y "Backspace")
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

## Sistema de Modificadores de Tecla

Puede usar los siguientes modificadores para activar los escucha de eventos del mouse o del teclado solo cuando se presiona la tecla modificadora correspondiente:

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

::: tip Nota
En teclados Macintosh, 'meta' es la tecla command (⌘). En teclados Windows, 'meta' es la tecla Windows (⊞). En teclados Sun Microsystems, meta está marcado como un diamánte sólido (◆).
En ciertos teclados, especialmente en teclados mecánicos MIT, LISP y sucesores, como el teclado Knight, el telcado space-cadet, meta está etiquetado como “META”. En los teclados Symbolics, meta está etiquetado como “META” o “Meta”.
:::

Por ejemplo:

```html
<!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Hacer algo</div>
```

::: tip
Tenga en cuenta que las teclas modificadoras son diferentes de las teclas normales y cuando se usa con eventos `keyup`, deben presionarse cuando se emite el evento. En otras palabras, `keyup.ctrl` solo se activará si se suelta una tecla mientras mantiene presionado `ctrl`. No se activará si suelta la tecla `ctrl` sola.
:::

### `.exact` Modifier

El modificador `.exact` permite el control de la combinación exacta de modificadores del sistema necesarios para desencadenar un evento.

```html
<!-- esto se disparará incluso si también se presiona Alt o Shift -->
<button @click.ctrl="onClick">A</button>

<!-- esto solo se activará cuando se presione Ctrl y no se presionen otras teclas -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- esto solo se disparará cuando no se presionen modificadores de sistema -->
<button @click.exact="onClick">A</button>
```

### Modificadores para Botones del Mouse

- `.left`
- `.right`
- `.middle`

Estos modificadores restringen que se active el manejador de eventos por un botón del mouse en específico.


## Por qué Listeners en HTML?

Es posible que le preocupe que todo este enfoque de escucha de eventos viole las viejas reglas sobre la "separación de responsabilidades". Tenga la seguridad - dado que todas las funciones de manejo y expresiones de Vue están estrictamente vinculadas al ViewModel que maneja la vista actual, no causará ninguna dificultad de mantenimiento. De hecho, existen varios beneficios al usar `v-on` o `@`:

1. Es más fácil ubicar las implementaciones de la función _handler_ dentro de su código JS examinando la plantilla HTML.

2. Dado que no tiene que adjuntar manualmente escucha de eventos en JS, su código ViewModel puede ser pura lógica y libre de DOM. Esto facilita la prueba.

Cuando se destruye un ViewModel, todos los escucha de eventos se eliminan automáticamente. No necesita preocuparse por limpiarlo usted mismo.
