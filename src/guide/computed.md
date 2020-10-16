# Propiedades Computadas y Observadores

## Propiedades Computadas

Las expresiones en el _template_ son muy convenientes, pero están diseñadas para operaciones simples. Poner demasiada lógica en sus _templates_ puede hacerlos grandes, complejos y difíciles de mantener. Por ejemplo, si tenemos un objeto con un arreglo anidado:

```js
Vue.createApp({
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Guía Avanzada',
          'Vue 3 - Guía Básica',
          'Vue 4 - El Misterio'
        ]
      }
    }
  }
})
```

Y queremos mostrar diferentes mensajes dependiendo de si `author` ya tiene o no algunos libros

```html
<div id="computed-basics">
  <p>Tiene libros publicados:</p>
  <span>{{ author.books.length > 0 ? 'Sí' : 'No' }}</span>
</div>
```

En este punto, el _template_ ya no es simple y declarativo. Debe mirarlo por un segundo antes de darse cuenta de que realiza un cálculo dependiendo de `author.books`. El problema se agrava cuando desea incluir este cálculo en su _template_ más de una vez.

Es por eso que para cualquier lógica compleja que incluya datos reactivos, debería usar una **propiedad computada**.

### Ejemplo Básico

```html
<div id="computed-basics">
  <p>Tiene libros publicados:</p>
  <span>{{ publishedBooksMessage }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Guía Avanzada',
          'Vue 3 - Guía Básica',
          'Vue 4 - El Misterio'
        ]
      }
    }
  },
  computed: {
    // un getter computado
    publishedBooksMessage() {
      // `this` apunta a la instancia vm
      return this.author.books.length > 0 ? 'Sí' : 'No'
    }
  }
}).mount('#computed-basics')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="NWqzrjr" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Computed basic example">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/NWqzrjr">
  Computed basic example</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Aquí hemos declarado una propiedad computada `publishedBooksMessage`.

Intente cambiar el valor del arreglo `books` en el objeto de datos `data` de la aplicación y observará cómo cambia `publishedBooksMessage` en consecuencia.

Puede enlazar datos a propiedades computadas en el _template_ al igual que una propiedad normal. Vue es consciente de que `vm.publishedBooksMessage` depende de `vm.author.books`, por lo cual actualizará todos los enlaces que dependan de `vm.publishedBooksMessage` cuando `vm.author.books` cambie. Y lo mejor de todo es que hemos creado esta relación de dependencia de manera declarativa: la función computada _getter_ no tiene efectos secundarios, lo que facilita la prueba y la comprensión.

### Caching Computado vs Métodos

Es posible que haya notado que podemos lograr el mismo resultado al invocar un método en la expresión:

```html
<p>{{ calculateBooksMessage() }}</p>
```

```js
// en el componente
methods: {
  calculateBooksMessage() {
    return this.author.books.length > 0 ? 'Sí' : 'No'
  }
}
```

En lugar de una propiedad computada, podemos definir la misma función como un método en su lugar. Para el resultado final, los dos enfoques son exactamente los mismos. Sin embargo, la diferencia es que las **propiedades computadas se almacenan en caché según sus dependencias**. Una propiedad computada solo se volverá a evaluar cuando alguna de sus dependencias haya cambiado. Esto significa que mientras `author.books` no haya cambiado, el acceso múltiple a la propiedad computada de `publishedBooksMessage` regresará inmediatamente el resultado previamente calculado sin tener que ejecutar la función de nuevo.

Esto también significa que la siguiente propiedad computada nunca se actualizará, porque `Date.now()` no es una dependencia reactiva:

```js
computed: {
  now() {
    return Date.now()
  }
}
```

En comparación, una invocación de método **siempre** ejecutará la función cada vez que ocurre una re-renderización.

¿Por qué necesitamos caché? Imagine que tenemos una costosa propiedad computada `list`, que requiere hacer un bucle a través de un gran arreglo y hace muchos cálculos. Entonces podemos tener otras propiedades computadas que a su vez dependen de `list`. Sin caché, ¡estaríamos ejecutando el _getter_ de `list` muchas veces más de lo necesario!. En los casos en que no desee el almacenamiento en caché, utilice un `method` en su lugar.

### Setter Computado

Las propiedades computadas son, de forma predeterminada solo _get_, pero también puede proporcionar un _set_ cuando lo necesite:

```js
// ...
computed: {
  fullName: {
    // getter
    get() {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set(newValue) {
      const names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Ahora, cuando ejecute `vm.fullName = 'John Doe'`, se invocará el _setter_ y `vm.firstName` y `vm.lastName` se actualizarán en consecuencia.

## Watchers

Si bien las propiedades computadas son más apropiadas en la mayoría de los casos, hay ocasiones en que es necesario un observador personalizado. Es por eso que Vue proporciona una forma más genérica de reaccionar a los cambios de datos a través de la opción `watch`. Esto es más útil cuando desea realizar operaciones asíncronas o costosas en respuesta al cambio de datos.

Por ejemplo:

```html
<div id="watch-example">
  <p>
    Haga una pregunta de sí/no:
    <input v-model="question" />
  </p>
  <p>{{ answer }}</p>
</div>
```

```html
<!-- Dado que ya existe un rico ecosistema de bibliotecas ajax         -->
<!-- y colecciones de métodos de utilidad de uso general, Vue core     -->
<!-- es capaz de permanecer pequeño al no reinventarlos. Esto también  -->
<!-- le da la libertad de usar aquello con lo que estás familiarizado. -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script>
  const watchExampleVM = Vue.createApp({
    data() {
      return {
        question: '',
        answer: 'Las preguntas usualmente contienen un signo de interrogación. ;-)'
      }
    },
    watch: {
      // cada vez que la pregunta cambie, esta función será ejecutada
      question(newQuestion, oldQuestion) {
        if (newQuestion.indexOf('?') > -1) {
          this.getAnswer()
        }
      }
    },
    methods: {
      getAnswer() {
        this.answer = 'Procesando...'
        axios
          .get('https://yesno.wtf/api')
          .then(response => {
            this.answer = response.data.answer
          })
          .catch(error => {
            this.answer = '¡Error! No se pudo comunicar con la API. ' + error
          })
      }
    }
  }).mount('#watch-example')
</script>
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="GRJGqXp" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Watch basic example">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/GRJGqXp">
  Watch basic example</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

En este caso, el uso de la opción `watch` nos permite realizar una operación asíncrona (acceder a una API) y define una condición para realizar esta operación. Nada de eso sería posible con una propiedad computada.

Además de la opción `watch`, también puede usar el imperativo [vm.\$watch API](../api/instance-methods.html#watch).

### Propiedad Computada vs Watched

Vue proporciona una forma más genérica de observar y reaccionar a los cambios de datos en una instancia activa: **propiedades _watch_**. Cuando tiene algunos datos que necesitan cambiarse en función de otros datos, es tentador utilizar `watch` en exceso, especialmente si proviene de tener experiencia en AngularJS. Sin embargo, a menudo es una mejor idea usar una propiedad computada en lugar de una imperativa llamada a `watch`. Considere este ejemplo:

```html
<div id="demo">{{ fullName }}</div>
```

```js
const vm = Vue.createApp({
  data() {
    return {
      firstName: 'Foo',
      lastName: 'Bar',
      fullName: 'Foo Bar'
    }
  },
  watch: {
    firstName(val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName(val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
}).mount('#demo')
```

El código anterior es imperativo y repetitivo. Compárelo con una versión de propiedad computada:

```js
const vm = Vue.createApp({
  data() {
    return {
      firstName: 'Foo',
      lastName: 'Bar'
    }
  },
  computed: {
    fullName() {
      return this.firstName + ' ' + this.lastName
    }
  }
}).mount('#demo')
```

Mucho mejor, ¿no?
