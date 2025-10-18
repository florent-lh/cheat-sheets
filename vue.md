# Vue.js 3 CheatSheet

## Table des matières
- [Installation](#installation)
- [Création d'application](#création-dapplication)
- [Composition API](#composition-api)
- [Options API](#options-api)
- [Template Syntax](#template-syntax)
- [Directives](#directives)
- [Réactivité](#réactivité)
- [Computed & Watchers](#computed--watchers)
- [Lifecycle Hooks](#lifecycle-hooks)
- [Composables](#composables)
- [Props, Emits & Slots](#props-emits--slots)
- [Vue Router](#vue-router)
- [Pinia (State Management)](#pinia-state-management)
- [Teleport & Suspense](#teleport--suspense)
- [TypeScript](#typescript)
- [Performance & Optimisation](#performance--optimisation)

---

## Installation

### Via npm/yarn/pnpm

```bash
# npm
npm create vue@latest

# yarn
yarn create vue

# pnpm
pnpm create vue
```

### CDN

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

### Vite (recommandé)

```bash
npm create vite@latest my-vue-app -- --template vue
cd my-vue-app
npm install
npm run dev
```

---

## Création d'application

### Avec Composition API

```javascript
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.mount('#app')
```

### Configuration globale

```javascript
app.config.globalProperties.$api = apiService
app.provide('key', 'value')
app.use(router)
app.use(pinia)
app.component('GlobalComponent', Component)
app.directive('focus', {
  mounted(el) { el.focus() }
})
```

---

## Composition API

### Structure de base (script setup) ⭐ NOUVEAU

```vue
<script setup>
import { ref, computed, onMounted } from 'vue'

// Variables réactives
const count = ref(0)
const message = ref('Hello')

// Computed
const doubleCount = computed(() => count.value * 2)

// Methods
function increment() {
  count.value++
}

// Lifecycle
onMounted(() => {
  console.log('Component mounted')
})

// Exposer pour template refs (optionnel)
defineExpose({
  increment
})
</script>

<template>
  <div>
    <p>{{ count }} - {{ doubleCount }}</p>
    <button @click="increment">+</button>
  </div>
</template>
```

### Props et Emits avec TypeScript

```vue
<script setup lang="ts">
interface Props {
  title: string
  count?: number
  items: string[]
}

// Props
const props = withDefaults(defineProps<Props>(), {
  count: 0
})

// Emits
const emit = defineEmits<{
  update: [value: number]
  delete: [id: string]
}>()

// Ou avec validation
const emit = defineEmits({
  update: (value: number) => value >= 0,
  delete: (id: string) => typeof id === 'string'
})

function handleUpdate() {
  emit('update', props.count + 1)
}
</script>
```

### defineModel ⭐ NOUVEAU (Vue 3.4+)

```vue
<script setup>
// Plus besoin de définir props + emit pour v-model
const model = defineModel() // Crée automatiquement v-model
const count = defineModel('count', { default: 0 })

function increment() {
  count.value++
}
</script>

<template>
  <input v-model="model" />
  <button @click="increment">Count: {{ count }}</button>
</template>
```

### Composition API classique (sans setup)

```vue
<script>
import { ref, computed, onMounted } from 'vue'

export default {
  setup(props, { emit, slots, attrs, expose }) {
    const count = ref(0)
    
    const doubleCount = computed(() => count.value * 2)
    
    function increment() {
      count.value++
      emit('update', count.value)
    }
    
    onMounted(() => {
      console.log('mounted')
    })
    
    // Retourner ce qui est exposé au template
    return {
      count,
      doubleCount,
      increment
    }
  }
}
</script>
```

---

## Options API

### Structure complète

```vue
<script>
export default {
  name: 'MyComponent',
  
  // Props
  props: {
    title: {
      type: String,
      required: true,
      validator: (value) => value.length > 0
    },
    count: {
      type: Number,
      default: 0
    }
  },
  
  // Emits
  emits: ['update', 'delete'],
  
  // Data
  data() {
    return {
      message: 'Hello',
      items: []
    }
  },
  
  // Computed
  computed: {
    reversedMessage() {
      return this.message.split('').reverse().join('')
    },
    
    // Avec setter
    fullName: {
      get() {
        return `${this.firstName} ${this.lastName}`
      },
      set(value) {
        [this.firstName, this.lastName] = value.split(' ')
      }
    }
  },
  
  // Methods
  methods: {
    handleClick() {
      this.$emit('update', this.count)
    }
  },
  
  // Watchers
  watch: {
    message(newVal, oldVal) {
      console.log(newVal, oldVal)
    },
    
    // Deep watch
    user: {
      handler(newVal) {
        console.log(newVal)
      },
      deep: true,
      immediate: true
    }
  },
  
  // Lifecycle hooks
  beforeCreate() {},
  created() {},
  beforeMount() {},
  mounted() {},
  beforeUpdate() {},
  updated() {},
  beforeUnmount() {},
  unmounted() {}
}
</script>
```

---

## Template Syntax

### Interpolation

```vue
<template>
  <!-- Texte -->
  <p>{{ message }}</p>
  
  <!-- JavaScript expressions -->
  <p>{{ count + 1 }}</p>
  <p>{{ ok ? 'YES' : 'NO' }}</p>
  <p>{{ message.split('').reverse().join('') }}</p>
  
  <!-- HTML brut (attention XSS) -->
  <div v-html="rawHtml"></div>
  
  <!-- Attributs -->
  <div :id="dynamicId"></div>
  <button :disabled="isDisabled">Button</button>
  
  <!-- Binding multiple attributes -->
  <div v-bind="objectOfAttrs"></div>
</template>
```

### Expressions et fonctions

```vue
<template>
  <!-- Appel de méthode -->
  <p>{{ formatDate(date) }}</p>
  
  <!-- Pas de statements -->
  <!-- ❌ <p>{{ var a = 1 }}</p> -->
  
  <!-- Opérateur ternaire OK -->
  <p>{{ user ? user.name : 'Guest' }}</p>
  
  <!-- Chaînage optionnel -->
  <p>{{ user?.profile?.email }}</p>
</template>
```

---

## Directives

### v-bind (ou :)

```vue
<template>
  <!-- Attribut simple -->
  <img :src="imageSrc" :alt="imageAlt">
  
  <!-- Classe -->
  <div :class="{ active: isActive, 'text-danger': hasError }"></div>
  <div :class="[activeClass, errorClass]"></div>
  <div :class="[isActive ? activeClass : '', errorClass]"></div>
  
  <!-- Style -->
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  <div :style="[baseStyles, overridingStyles]"></div>
  
  <!-- Props dynamiques ⭐ -->
  <component :[attributeName]="value"></component>
  
  <!-- Modificateurs -->
  <div :class.camel="myClass"></div>
  <div :style.prop="{ color: 'red' }"></div>
</template>
```

### v-on (ou @)

```vue
<template>
  <!-- Événements -->
  <button @click="handleClick">Click</button>
  <button @click="count++">Increment</button>
  <button @click="handleClick($event, 'arg')">With args</button>
  
  <!-- Modificateurs d'événement -->
  <form @submit.prevent="onSubmit"></form>
  <input @keyup.enter="submit">
  <div @click.stop="doThis"></div>
  <div @click.once="doThis"></div>
  <div @click.capture="doThis"></div>
  
  <!-- Modificateurs de clavier -->
  <input @keyup.enter="submit">
  <input @keyup.tab="nextField">
  <input @keyup.delete="deleteItem">
  <input @keyup.esc="cancel">
  <input @keyup.space="doSomething">
  <input @keyup.up="scrollUp">
  <input @keyup.down="scrollDown">
  <input @keyup.left="previous">
  <input @keyup.right="next">
  
  <!-- Combinaisons -->
  <input @keyup.ctrl.enter="submit">
  <div @click.ctrl="doSomething">Ctrl + Click</div>
  
  <!-- Modificateurs de souris -->
  <div @click.left="handleLeft"></div>
  <div @click.right.prevent="handleRight"></div>
  <div @click.middle="handleMiddle"></div>
  
  <!-- Événement dynamique -->
  <button @[eventName]="handler">Dynamic</button>
</template>
```

### v-model

```vue
<template>
  <!-- Input texte -->
  <input v-model="text">
  
  <!-- Textarea -->
  <textarea v-model="message"></textarea>
  
  <!-- Checkbox (booléen) -->
  <input type="checkbox" v-model="checked">
  
  <!-- Checkbox multiple (array) -->
  <input type="checkbox" value="A" v-model="checkedNames">
  <input type="checkbox" value="B" v-model="checkedNames">
  
  <!-- Radio -->
  <input type="radio" value="One" v-model="picked">
  <input type="radio" value="Two" v-model="picked">
  
  <!-- Select -->
  <select v-model="selected">
    <option disabled value="">Choisir</option>
    <option>A</option>
    <option>B</option>
  </select>
  
  <!-- Select multiple -->
  <select v-model="selectedMultiple" multiple>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  
  <!-- Modificateurs -->
  <input v-model.lazy="text">      <!-- sync on change -->
  <input v-model.number="age">     <!-- cast to number -->
  <input v-model.trim="message">   <!-- trim whitespace -->
  
  <!-- v-model personnalisé sur composant -->
  <CustomInput v-model="value" />
  <CustomInput v-model:title="bookTitle" />
</template>
```

### v-if / v-else-if / v-else

```vue
<template>
  <div v-if="type === 'A'">A</div>
  <div v-else-if="type === 'B'">B</div>
  <div v-else>Not A or B</div>
  
  <!-- Grouper avec template -->
  <template v-if="ok">
    <h1>Title</h1>
    <p>Paragraph</p>
  </template>
</template>
```

### v-show

```vue
<template>
  <!-- Display: none au lieu de remove du DOM -->
  <div v-show="isVisible">Visible</div>
</template>
```

### v-for

```vue
<template>
  <!-- Array -->
  <li v-for="item in items" :key="item.id">
    {{ item.name }}
  </li>
  
  <!-- Avec index -->
  <li v-for="(item, index) in items" :key="item.id">
    {{ index }} - {{ item.name }}
  </li>
  
  <!-- Object -->
  <div v-for="(value, key) in object" :key="key">
    {{ key }}: {{ value }}
  </div>
  
  <!-- Avec index d'objet -->
  <div v-for="(value, key, index) in object" :key="key">
    {{ index }}. {{ key }}: {{ value }}
  </div>
  
  <!-- Range -->
  <span v-for="n in 10" :key="n">{{ n }}</span>
  
  <!-- Sur template -->
  <template v-for="item in items" :key="item.id">
    <li>{{ item.name }}</li>
    <li class="divider"></li>
  </template>
  
  <!-- v-for avec v-if (pas recommandé ensemble) -->
  <li v-for="todo in todos" :key="todo.id" v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
  
  <!-- Meilleur : computed filtré -->
  <li v-for="todo in activeTodos" :key="todo.id">
    {{ todo.name }}
  </li>
</template>
```

### Autres directives

```vue
<template>
  <!-- v-text -->
  <span v-text="message"></span>
  
  <!-- v-html (attention XSS!) -->
  <div v-html="rawHtml"></div>
  
  <!-- v-pre (skip compilation) -->
  <span v-pre>{{ this will not be compiled }}</span>
  
  <!-- v-once (render once) -->
  <span v-once>{{ message }}</span>
  
  <!-- v-memo ⭐ NOUVEAU (Vue 3.2+) -->
  <!-- Re-render seulement si valueA ou valueB change -->
  <div v-memo="[valueA, valueB]">
    <!-- contenu -->
  </div>
  
  <!-- v-cloak (cacher jusqu'à ready) -->
  <div v-cloak>{{ message }}</div>
</template>

<style>
[v-cloak] { display: none; }
</style>
```

### Directives personnalisées

```javascript
// Directive globale
app.directive('focus', {
  mounted(el) {
    el.focus()
  }
})

// Directive locale
const vFocus = {
  mounted: (el) => el.focus()
}

// Utilisation
<input v-focus>

// Avec argument et modificateurs
app.directive('color', {
  mounted(el, binding) {
    // binding.value - valeur passée
    // binding.arg - argument (v-color:background)
    // binding.modifiers - modificateurs (v-color.important)
    el.style.color = binding.value
  },
  updated(el, binding) {
    el.style.color = binding.value
  }
})

// Usage: <p v-color="'red'">Text</p>
```

---

## Réactivité

### ref()

```javascript
import { ref } from 'vue'

// Primitives
const count = ref(0)
const message = ref('hello')

// Accès et modification
console.log(count.value) // 0
count.value++

// Objets (déballé automatiquement)
const obj = ref({ count: 0 })
obj.value.count++ // OK
```

### reactive()

```javascript
import { reactive } from 'vue'

// Pour les objets seulement
const state = reactive({
  count: 0,
  user: {
    name: 'John',
    age: 30
  }
})

// Pas besoin de .value
state.count++
state.user.name = 'Jane'

// ⚠️ Ne pas déstructurer (perd réactivité)
let { count } = state // ❌ Non réactif
const { count } = toRefs(state) // ✅ Réactif
```

### computed()

```javascript
import { ref, computed } from 'vue'

const count = ref(0)

// Lecture seule
const doubleCount = computed(() => count.value * 2)

// Avec getter et setter
const fullName = computed({
  get: () => `${firstName.value} ${lastName.value}`,
  set: (value) => {
    [firstName.value, lastName.value] = value.split(' ')
  }
})
```

### readonly()

```javascript
import { reactive, readonly } from 'vue'

const original = reactive({ count: 0 })
const copy = readonly(original)

// original.count++ ✅ OK
// copy.count++ ❌ Warning
```

### watchEffect() et watch()

```javascript
import { ref, watch, watchEffect } from 'vue'

const count = ref(0)
const user = reactive({ name: 'John', age: 30 })

// watchEffect - exécute immédiatement et réagit aux dépendances
watchEffect(() => {
  console.log(`Count is: ${count.value}`)
})

// watchEffect avec cleanup
watchEffect((onCleanup) => {
  const token = performAsyncOperation()
  onCleanup(() => {
    token.cancel()
  })
})

// watch - plus de contrôle
watch(count, (newValue, oldValue) => {
  console.log(`Count: ${oldValue} -> ${newValue}`)
})

// watch multiple sources
watch([count, () => user.name], ([newCount, newName], [oldCount, oldName]) => {
  console.log(`Count: ${oldCount} -> ${newCount}`)
  console.log(`Name: ${oldName} -> ${newName}`)
})

// Options
watch(
  source,
  callback,
  {
    immediate: true,    // Exécuter immédiatement
    deep: true,         // Watch profond
    flush: 'post',      // 'pre' | 'post' | 'sync'
    onTrack(e) {},      // Debug
    onTrigger(e) {}     // Debug
  }
)

// Arrêter un watcher
const stop = watchEffect(() => {})
stop() // Arrête le watcher
```

### toRef() et toRefs()

```javascript
import { reactive, toRef, toRefs } from 'vue'

const state = reactive({
  foo: 1,
  bar: 2
})

// toRef - créer un ref vers une propriété
const fooRef = toRef(state, 'foo')
fooRef.value++ // state.foo est maintenant 2

// toRefs - convertir tout l'objet
const { foo, bar } = toRefs(state)
// foo et bar sont des refs liés à state
foo.value++
```

### unref() et isRef()

```javascript
import { ref, unref, isRef } from 'vue'

const count = ref(0)

// unref - obtenir la valeur
const value = unref(count) // 0
const value2 = unref(5)    // 5 (si pas ref)

// isRef - vérifier si ref
console.log(isRef(count)) // true
console.log(isRef(0))     // false
```

### shallowRef() et shallowReactive() ⭐

```javascript
import { shallowRef, shallowReactive } from 'vue'

// Réactivité superficielle uniquement
const state = shallowRef({ count: 0 })
state.value = { count: 1 } // ✅ Trigger
state.value.count = 1      // ❌ Ne trigger pas

const state2 = shallowReactive({ nested: { count: 0 } })
state2.nested = { count: 1 } // ✅ Trigger
state2.nested.count = 1      // ❌ Ne trigger pas
```

### triggerRef() ⭐

```javascript
import { shallowRef, triggerRef } from 'vue'

const state = shallowRef({ count: 0 })
state.value.count++
triggerRef(state) // Force l'update
```

### customRef() - Réactivité personnalisée

```javascript
import { customRef } from 'vue'

function useDebouncedRef(value, delay = 200) {
  let timeout
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
          value = newValue
          trigger()
        }, delay)
      }
    }
  })
}

const text = useDebouncedRef('hello')
```

---

## Computed & Watchers

### Computed avancé

```javascript
import { ref, computed } from 'vue'

const count = ref(0)

// Computed avec cache
const expensiveComputation = computed(() => {
  console.log('Computing...')
  return count.value * 2
})

// Computed writable
const fullName = computed({
  get() {
    return `${firstName.value} ${lastName.value}`
  },
  set(value) {
    [firstName.value, lastName.value] = value.split(' ')
  }
})
```

### Watch avancé

```javascript
import { ref, watch } from 'vue'

// Watch avec getter
const obj = reactive({ count: 0 })
watch(
  () => obj.count,
  (count) => console.log(count)
)

// Deep watch
watch(
  obj,
  (newValue, oldValue) => {
    // Note: newValue === oldValue pour objets
  },
  { deep: true }
)

// Multiple sources
watch(
  [refA, () => refB.value],
  ([a, b], [prevA, prevB]) => {
    // ...
  }
)

// Flush timing
watch(source, callback, {
  flush: 'post' // 'pre' (défaut) | 'post' | 'sync'
})

// watchPostEffect et watchSyncEffect ⭐ NOUVEAU
import { watchPostEffect, watchSyncEffect } from 'vue'

watchPostEffect(() => {
  // Exécuté après les updates du DOM
})

watchSyncEffect(() => {
  // Exécuté de façon synchrone
})
```

---

## Lifecycle Hooks

### Composition API

```javascript
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  onActivated,      // keep-alive
  onDeactivated     // keep-alive
} from 'vue'

export default {
  setup() {
    // ⚠️ Pas de beforeCreate ni created (setup = created)
    
    onBeforeMount(() => {
      console.log('Before mount')
    })
    
    onMounted(() => {
      console.log('Mounted')
      // Accès au DOM
    })
    
    onBeforeUpdate(() => {
      console.log('Before update')
    })
    
    onUpdated(() => {
      console.log('Updated')
    })
    
    onBeforeUnmount(() => {
      console.log('Before unmount')
      // Cleanup
    })
    
    onUnmounted(() => {
      console.log('Unmounted')
    })
    
    onErrorCaptured((err, instance, info) => {
      console.error(err)
      return false // Empêche la propagation
    })
    
    // Pour keep-alive
    onActivated(() => {
      console.log('Activated')
    })
    
    onDeactivated(() => {
      console.log('Deactivated')
    })
  }
}
```

### Options API vs Composition API

| Options API | Composition API |
|------------|----------------|
| beforeCreate | setup() |
| created | setup() |
| beforeMount | onBeforeMount |
| mounted | onMounted |
| beforeUpdate | onBeforeUpdate |
| updated | onUpdated |
| beforeUnmount | onBeforeUnmount |
| unmounted | onUnmounted |
| errorCaptured | onErrorCaptured |
| activated | onActivated |
| deactivated | onDeactivated |

### onServerPrefetch (SSR)

```javascript
import { onServerPrefetch } from 'vue'

onServerPrefetch(async () => {
  // Exécuté uniquement côté serveur
  data.value = await fetchData()
})
```

---

## Composables

### Structure d'un composable

```javascript
// composables/useMouse.js
import { ref, onMounted, onUnmounted } from 'vue'

export function useMouse() {
  const x = ref(0)
  const y = ref(0)

  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  onMounted(() => {
    window.addEventListener('mousemove', update)
  })

  onUnmounted(() => {
    window.removeEventListener('mousemove', update)
  })

  return { x, y }
}
```

### Utilisation

```vue
<script setup>
import { useMouse } from './composables/useMouse'

const { x, y } = useMouse()
</script>

<template>
  <p>Mouse position: {{ x }}, {{ y }}</p>
</template>
```

### Composable avec paramètres

```javascript
// composables/useFetch.js
import { ref, watchEffect, toValue } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)
  const loading = ref(false)

  watchEffect(async () => {
    loading.value = true
    data.value = null
    error.value = null
    
    try {
      // toValue() gère ref, getter ou valeur brute ⭐ NOUVEAU
      const response = await fetch(toValue(url))
      data.value = await response.json()
    } catch (e) {
      error.value = e
    } finally {
      loading.value = false
    }
  })

  return { data, error, loading }
}

// Usage
const url = ref('/api/users')
const { data, error, loading } = useFetch(url)
```

### Composable asynchrone

```javascript
// composables/useAsyncData.js
import { ref } from 'vue'

export async function useAsyncData(fetcher) {
  const data = ref(null)
  const error = ref(null)
  
  try {
    data.value = await fetcher()
  } catch (e) {
    error.value = e
  }
  
  return { data, error }
}

// Usage avec Suspense
const { data } = await useAsyncData(() => fetchUser(id))
```

### Composables communs

```javascript
// useCounter.js
export function useCounter(initial = 0) {
  const count = ref(initial)
  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initial
  
  return { count, increment, decrement, reset }
}

// useLocalStorage.js
export function useLocalStorage(key, initialValue) {
  const storedValue = ref(initialValue)
  
  onMounted(() => {
    const item = localStorage.getItem(key)
    if (item) {
      storedValue.value = JSON.parse(item)
    }
  })
  
  watch(storedValue, (newValue) => {
    localStorage.setItem(key, JSON.stringify(newValue))
  }, { deep: true })
  
  return storedValue
}

// useDebounce.js
export function useDebounce(fn, delay = 300) {
  let timeout
  return (...args) => {
    clearTimeout(timeout)
    timeout = setTimeout(() => fn(...args), delay)
  }
}
```

---

## Props, Emits & Slots

### Props (script setup)

```vue
<script setup>
// Props simples
const props = defineProps({
  title: String,
  count: Number,
  items: Array,
  user: Object,
  callback: Function
})

// Props avec options
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  },
  items: {
    type: Array,
    default: () => []
  },
  status: {
    type: String,
    validator: (value) => ['success', 'warning', 'error'].includes(value)
  }
})

// Props avec TypeScript ⭐
interface Props {
  title: string
  count?: number
  items: string[]
}

const props = withDefaults(defineProps<Props>(), {
  count: 0,
  items: () => []
})

// Accès aux props
console.log(props.title)
</script>
```

### Emits (script setup)

```vue
<script setup>
// Emits simples
const emit = defineEmits(['update', 'delete'])

// Avec validation
const emit = defineEmits({
  update: (value) => typeof value === 'number',
  delete: (id) => typeof id === 'string'
})

// TypeScript ⭐
const emit = defineEmits<{
  update: [id: number, value: string]
  delete: [id: number]
}>()

// Émettre des événements
function handleClick() {
  emit('update', 42, 'hello')
}
</script>
```

### v-model personnalisé

```vue
<!-- ParentComponent.vue -->
<template>
  <CustomInput v-model="searchText" />
  
  <!-- Équivalent à -->
  <CustomInput
    :modelValue="searchText"
    @update:modelValue="searchText = $event"
  />
</template>

<!-- CustomInput.vue -->
<script setup>
defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
```

### Multiple v-models ⭐

```vue
<!-- ParentComponent.vue -->
<template>
  <UserForm
    v-model:first-name="first"
    v-model:last-name="last"
  />
</template>

<!-- UserForm.vue -->
<script setup>
defineProps(['firstName', 'lastName'])
const emit = defineEmits(['update:firstName', 'update:lastName'])
</script>

<template>
  <input
    :value="firstName"
    @input="emit('update:firstName', $event.target.value)"
  />
  <input
    :value="lastName"
    @input="emit('update:lastName', $event.target.value)"
  />
</template>
```

### Slots

```vue
<!-- ChildComponent.vue -->
<template>
  <div class="container">
    <!-- Slot par défaut -->
    <slot></slot>
    
    <!-- Slot nommé -->
    <header>
      <slot name="header"></slot>
    </header>
    
    <main>
      <slot></slot>
    </main>
    
    <footer>
      <slot name="footer"></slot>
    </footer>
    
    <!-- Slot avec fallback -->
    <slot name="optional">Contenu par défaut</slot>
    
    <!-- Scoped slot -->
    <slot name="item" :item="item" :index="index"></slot>
  </div>
</template>

<!-- ParentComponent.vue -->
<template>
  <ChildComponent>
    <!-- Contenu du slot par défaut -->
    <p>Contenu principal</p>
    
    <!-- Slots nommés -->
    <template #header>
      <h1>Titre</h1>
    </template>
    
    <template #footer>
      <p>Footer</p>
    </template>
    
    <!-- Scoped slot -->
    <template #item="{ item, index }">
      <li>{{ index }}: {{ item.name }}</li>
    </template>
    
    <!-- Déstructuration -->
    <template #item="{ item: { name, price } }">
      <li>{{ name }} - {{ price }}€</li>
    </template>
  </ChildComponent>
</template>
```

### useSlots() et useAttrs()

```vue
<script setup>
import { useSlots, useAttrs } from 'vue'

const slots = useSlots()
const attrs = useAttrs()

// Vérifier si un slot existe
if (slots.header) {
  console.log('Header slot exists')
}

// Accéder aux attributs non-props
console.log(attrs.class)
console.log(attrs.id)
</script>
```

### Provide / Inject

```vue
<!-- Parent.vue -->
<script setup>
import { provide, ref } from 'vue'

const theme = ref('dark')
provide('theme', theme)

// Avec clé Symbol (recommandé)
const ThemeKey = Symbol()
provide(ThemeKey, theme)

// Provide avec readonly
provide('readOnlyTheme', readonly(theme))
</script>

<!-- Enfant.vue (n'importe quel niveau) -->
<script setup>
import { inject } from 'vue'

// Avec valeur par défaut
const theme = inject('theme', 'light')

// TypeScript
const theme = inject<Ref<string>>('theme')

// Avec Symbol
const theme = inject(ThemeKey)
</script>
```

---

## Vue Router

### Installation

```bash
npm install vue-router@4
```

### Configuration de base

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // Lazy loading
    component: () => import('../views/About.vue')
  },
  {
    path: '/user/:id',
    name: 'User',
    component: () => import('../views/User.vue'),
    props: true // Passe params comme props
  },
  {
    path: '/user/:id(\\d+)', // Regex
    name: 'User',
    component: UserView
  },
  {
    // 404
    path: '/:pathMatch(.*)*',
    name: 'NotFound',
    component: NotFound
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  }
})

export default router
```

### Routes imbriquées

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        path: '', // /user/:id
        component: UserHome
      },
      {
        path: 'profile', // /user/:id/profile
        component: UserProfile
      },
      {
        path: 'posts', // /user/:id/posts
        component: UserPosts
      }
    ]
  }
]
```

### Navigation guards

```javascript
// Guard global
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuthenticated()) {
    next('/login')
  } else {
    next()
  }
})

router.afterEach((to, from) => {
  // Après navigation
})

// Guard par route
const routes = [
  {
    path: '/admin',
    component: Admin,
    beforeEnter: (to, from, next) => {
      // Guard spécifique à cette route
      if (isAdmin()) {
        next()
      } else {
        next('/login')
      }
    }
  }
]

// Guard dans composant (Options API)
export default {
  beforeRouteEnter(to, from, next) {
    // Avant que le composant soit créé
    next(vm => {
      // Accès à l'instance via vm
    })
  },
  beforeRouteUpdate(to, from, next) {
    // Quand la route change mais composant réutilisé
    next()
  },
  beforeRouteLeave(to, from, next) {
    // Avant de quitter
    const answer = window.confirm('Quitter?')
    next(answer)
  }
}
```

### Utilisation (Composition API)

```vue
<script setup>
import { useRouter, useRoute, onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'

const router = useRouter()
const route = useRoute()

// Navigation
function goToUser() {
  router.push('/user/123')
  router.push({ name: 'User', params: { id: 123 } })
  router.push({ path: '/user', query: { plan: 'private' } })
  router.replace('/home') // Sans historique
  router.go(-1) // Retour
  router.back() // Retour
  router.forward() // Avancer
}

// Accès aux params et query
console.log(route.params.id)
console.log(route.query.search)
console.log(route.hash)
console.log(route.fullPath)
console.log(route.meta)

// Guards dans Composition API
onBeforeRouteLeave((to, from) => {
  const answer = window.confirm('Quitter?')
  if (!answer) return false
})

onBeforeRouteUpdate(async (to, from) => {
  // Réagir au changement de paramètres
  if (to.params.id !== from.params.id) {
    userData.value = await fetchUser(to.params.id)
  }
})
</script>

<template>
  <!-- RouterLink -->
  <RouterLink to="/">Home</RouterLink>
  <RouterLink :to="{ name: 'User', params: { id: 123 } }">User</RouterLink>
  <RouterLink to="/about" active-class="active">About</RouterLink>
  <RouterLink to="/about" replace>About (replace)</RouterLink>
  
  <!-- RouterView -->
  <RouterView />
  
  <!-- Avec transition -->
  <RouterView v-slot="{ Component }">
    <Transition name="fade">
      <component :is="Component" />
    </Transition>
  </RouterView>
  
  <!-- RouterView nommé -->
  <RouterView name="sidebar" />
  <RouterView name="content" />
</template>
```

### Routes avec meta

```javascript
const routes = [
  {
    path: '/admin',
    component: Admin,
    meta: {
      requiresAuth: true,
      title: 'Admin Panel',
      roles: ['admin']
    }
  }
]

// Guard utilisant meta
router.beforeEach((to, from, next) => {
  document.title = to.meta.title || 'Default Title'
  
  if (to.meta.requiresAuth) {
    // Vérifier auth
  }
  
  next()
})
```

### Navigation programmatique avec TypeScript

```typescript
import { RouteRecordName } from 'vue-router'

const router = useRouter()

// Type-safe navigation
router.push({
  name: 'User' as RouteRecordName,
  params: { id: '123' }
})
```

---

## Pinia (State Management)

### Installation

```bash
npm install pinia
```

### Configuration

```javascript
// main.js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

### Définir un store (Composition API) ⭐ Recommandé

```javascript
// stores/counter.js
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  // State
  const count = ref(0)
  const name = ref('Eduardo')

  // Getters
  const doubleCount = computed(() => count.value * 2)

  // Actions
  function increment() {
    count.value++
  }
  
  async function fetchData() {
    const data = await fetch('/api/data')
    count.value = await data.json()
  }

  return {
    count,
    name,
    doubleCount,
    increment,
    fetchData
  }
})
```

### Définir un store (Options API)

```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  // State
  state: () => ({
    count: 0,
    name: 'Eduardo'
  }),

  // Getters
  getters: {
    doubleCount: (state) => state.count * 2,
    
    // Accès à d'autres getters
    doubleCountPlusOne() {
      return this.doubleCount + 1
    },
    
    // Avec paramètres (retourner une fonction)
    getUserById: (state) => (id) => {
      return state.users.find(user => user.id === id)
    }
  },

  // Actions
  actions: {
    increment() {
      this.count++
    },
    
    async fetchData() {
      const data = await fetch('/api/data')
      this.count = await data.json()
    },
    
    // Accès à d'autres stores
    async orderCart() {
      const cartStore = useCartStore()
      const items = cartStore.items
      // ...
    }
  }
})
```

### Utilisation dans les composants

```vue
<script setup>
import { useCounterStore } from '@/stores/counter'
import { storeToRefs } from 'pinia'

const store = useCounterStore()

// ❌ Ne pas déstructurer directement (perd réactivité)
// const { count, name } = store

// ✅ Utiliser storeToRefs pour state et getters
const { count, name, doubleCount } = storeToRefs(store)

// ✅ Actions peuvent être déstructurées directement
const { increment, fetchData } = store

// Modification directe (OK pour Pinia)
store.count++

// Patch (modifier plusieurs propriétés)
store.$patch({
  count: store.count + 1,
  name: 'New Name'
})

// Patch avec fonction
store.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})

// Remplacer tout le state
store.$state = { count: 0, name: 'Eduardo' }

// Reset
store.$reset()
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double: {{ doubleCount }}</p>
    <button @click="increment">+</button>
    <button @click="store.count++">+ Direct</button>
  </div>
</template>
```

### Persist plugin

```javascript
// Installation
npm install pinia-plugin-persistedstate

// main.js
import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)

// Store avec persistance
export const useUserStore = defineStore('user', () => {
  const name = ref('John')
  const age = ref(30)
  
  return { name, age }
}, {
  persist: true // Sauvegarde automatiquement dans localStorage
})

// Configuration avancée
export const useUserStore = defineStore('user', () => {
  // ...
}, {
  persist: {
    key: 'my-custom-key',
    storage: sessionStorage,
    paths: ['name'] // Persister seulement certaines propriétés
  }
})
```

### Subscribe aux changements

```javascript
const store = useCounterStore()

// Subscribe au state
store.$subscribe((mutation, state) => {
  console.log(mutation.type) // 'direct' | 'patch object' | 'patch function'
  console.log(mutation.storeId)
  console.log(mutation.payload) // Patch object pour $patch()
  console.log(state)
  
  // Persister dans localStorage
  localStorage.setItem('cart', JSON.stringify(state))
})

// Subscribe aux actions
store.$onAction(({
  name,    // nom de l'action
  store,   // instance du store
  args,    // paramètres passés à l'action
  after,   // hook après l'action
  onError  // hook si erreur
}) => {
  console.log(`Action ${name} called with`, args)
  
  after((result) => {
    console.log('Action finished with result:', result)
  })
  
  onError((error) => {
    console.error('Action failed:', error)
  })
})
```

---

## Teleport & Suspense

### Teleport ⭐

```vue
<template>
  <div>
    <button @click="open = true">Ouvrir modal</button>
    
    <!-- Téléporte le contenu vers body -->
    <Teleport to="body">
      <div v-if="open" class="modal">
        <p>Modal content</p>
        <button @click="open = false">Fermer</button>
      </div>
    </Teleport>
    
    <!-- Vers un sélecteur CSS -->
    <Teleport to="#modals">
      <div>Content</div>
    </Teleport>
    
    <!-- Désactiver conditionnellement -->
    <Teleport to="body" :disabled="isMobile">
      <div>Content</div>
    </Teleport>
  </div>
</template>
```

### Suspense ⭐ (Experimental)

```vue
<!-- App.vue -->
<template>
  <Suspense>
    <!-- Composant avec async setup -->
    <template #default>
      <AsyncComponent />
    </template>
    
    <!-- Fallback pendant le chargement -->
    <template #fallback>
      <div>Chargement...</div>
    </template>
  </Suspense>
</template>

<!-- AsyncComponent.vue -->
<script setup>
// Top-level await dans script setup
const data = await fetch('/api/data').then(r => r.json())

// Ou avec composable async
const { data } = await useFetch('/api/data')
</script>

<template>
  <div>{{ data }}</div>
</template>
```

### Suspense avec gestion d'erreur

```vue
<template>
  <RouterView v-slot="{ Component }">
    <Suspense>
      <template #default>
        <component :is="Component" />
      </template>
      
      <template #fallback>
        <LoadingSpinner />
      </template>
    </Suspense>
  </RouterView>
</template>

<script setup>
import { onErrorCaptured } from 'vue'

const error = ref(null)

onErrorCaptured((err) => {
  error.value = err
  return true // Empêche la propagation
})
</script>
```

---

## TypeScript

### Configuration de base

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "jsx": "preserve",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,
    "types": ["vite/client"]
  }
}
```

### Composant typé

```vue
<script setup lang="ts">
import { ref, computed, type Ref, type ComputedRef } from 'vue'

// Types de base
const count: Ref<number> = ref(0)
const message = ref<string>('hello') // Inférence automatique

// Interface
interface User {
  id: number
  name: string
  email?: string
}

const user = ref<User>({
  id: 1,
  name: 'John'
})

// Computed typé
const doubleCount: ComputedRef<number> = computed(() => count.value * 2)

// Fonction typée
function increment(amount: number = 1): void {
  count.value += amount
}

// Props typés
interface Props {
  title: string
  count?: number
  items: string[]
  user?: User
}

const props = withDefaults(defineProps<Props>(), {
  count: 0,
  user: () => ({ id: 0, name: 'Guest' })
})

// Emits typés
const emit = defineEmits<{
  update: [id: number, value: string]
  delete: [id: number]
}>()

// Refs de template typés
import type { ComponentPublicInstance } from 'vue'

const inputRef = ref<HTMLInputElement | null>(null)
const childRef = ref<ComponentPublicInstance | null>(null)

onMounted(() => {
  inputRef.value?.focus()
  childRef.value?.someMethod()
})
</script>
```

### Reactive typé

```typescript
import { reactive } from 'vue'

interface State {
  count: number
  user: User | null
}

const state: State = reactive({
  count: 0,
  user: null
})

// Ou
const state = reactive<State>({
  count: 0,
  user: null
})
```

### Composable typé

```typescript
// composables/useFetch.ts
import { ref, type Ref } from 'vue'

interface UseFetchReturn<T> {
  data: Ref<T | null>
  error: Ref<Error | null>
  loading: Ref<boolean>
  refetch: () => Promise<void>
}

export function useFetch<T>(url: string): UseFetchReturn<T> {
  const data = ref<T | null>(null)
  const error = ref<Error | null>(null)
  const loading = ref(false)

  async function fetchData() {
    loading.value = true
    try {
      const response = await fetch(url)
      data.value = await response.json()
    } catch (e) {
      error.value = e as Error
    } finally {
      loading.value = false
    }
  }

  fetchData()

  return {
    data,
    error,
    loading,
    refetch: fetchData
  }
}

// Usage
interface User {
  id: number
  name: string
}

const { data, error, loading } = useFetch<User>('/api/user')
// data est typé comme Ref<User | null>
```

### Store Pinia typé

```typescript
// stores/user.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

interface User {
  id: number
  name: string
  email: string
}

export const useUserStore = defineStore('user', () => {
  const user = ref<User | null>(null)
  const isLoggedIn = computed(() => user.value !== null)

  async function login(email: string, password: string): Promise<void> {
    const response = await fetch('/api/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    })
    user.value = await response.json()
  }

  function logout(): void {
    user.value = null
  }

  return {
    user,
    isLoggedIn,
    login,
    logout
  }
})
```

### Router typé

```typescript
// router/index.ts
import type { RouteRecordRaw } from 'vue-router'

declare module 'vue-router' {
  interface RouteMeta {
    requiresAuth?: boolean
    title?: string
    roles?: string[]
  }
}

const routes: RouteRecordRaw[] = [
  {
    path: '/admin',
    component: Admin,
    meta: {
      requiresAuth: true,
      title: 'Admin',
      roles: ['admin']
    }
  }
]
```

---

## Performance & Optimisation

### v-once - Render une fois

```vue
<template>
  <!-- Ne se met jamais à jour -->
  <span v-once>{{ message }}</span>
</template>
```

### v-memo - Memoization conditionnelle ⭐

```vue
<template>
  <!-- Re-render seulement si valueA ou valueB change -->
  <div v-memo="[valueA, valueB]">
    <ExpensiveComponent />
  </div>
  
  <!-- Utile dans v-for -->
  <div
    v-for="item in list"
    :key="item.id"
    v-memo="[item.id === selected]"
  >
    {{ item.name }}
  </div>
</template>
```

### Lazy loading composants

```javascript
import { defineAsyncComponent } from 'vue'

// Composant async basique
const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)

// Avec options
const AsyncComp = defineAsyncComponent({
  loader: () => import('./components/MyComponent.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
  delay: 200,
  timeout: 3000
})
```

### KeepAlive

```vue
<template>
  <!-- Cache les composants inactifs -->
  <KeepAlive>
    <component :is="activeComponent" />
  </KeepAlive>
  
  <!-- Avec include/exclude -->
  <KeepAlive :include="['ComponentA', 'ComponentB']">
    <component :is="view" />
  </KeepAlive>
  
  <KeepAlive :exclude="ComponentC">
    <component :is="view" />
  </KeepAlive>
  
  <!-- Limiter le nombre de caches -->
  <KeepAlive :max="10">
    <component :is="view" />
  </KeepAlive>
  
  <!-- Avec router -->
  <RouterView v-slot="{ Component }">
    <KeepAlive>
      <component :is="Component" />
    </KeepAlive>
  </RouterView>
</template>

<script setup>
import { onActivated, onDeactivated } from 'vue'

// Hooks spécifiques à KeepAlive
onActivated(() => {
  // Appelé quand le composant est réactivé
})

onDeactivated(() => {
  // Appelé quand le composant est désactivé
})
</script>
```

### shallowRef et shallowReactive

```javascript
import { shallowRef, shallowReactive, triggerRef } from 'vue'

// Réactivité superficielle uniquement
const state = shallowRef({ count: 0 })

// Ceci ne trigger pas de re-render
state.value.count++

// Mais ceci oui
state.value = { count: 1 }

// Forcer le re-render
state.value.count++
triggerRef(state)

// shallowReactive
const state2 = shallowReactive({
  nested: { count: 0 }
})

state2.nested = { count: 1 } // ✅ Réactif
state2.nested.count++         // ❌ Non réactif
```

### markRaw et effectScope

```javascript
import { reactive, markRaw, effectScope } from 'vue'

// markRaw - marquer un objet comme non-réactif
const state = reactive({
  foo: 1,
  bar: markRaw({
    // Cet objet ne sera jamais réactif
    nested: { count: 0 }
  })
})

// effectScope - grouper des effets
const scope = effectScope()

scope.run(() => {
  const doubled = computed(() => counter.value * 2)
  
  watch(doubled, () => console.log(doubled.value))
  
  watchEffect(() => console.log('Count: ', doubled.value))
})

// Dispose tous les effets d'un coup
scope.stop()
```

### Virtual Scrolling (avec vue-virtual-scroller)

```bash
npm install vue-virtual-scroller
```

```vue
<template>
  <RecycleScroller
    :items="items"
    :item-size="50"
    key-field="id"
    v-slot="{ item }"
  >
    <div class="item">{{ item.name }}</div>
  </RecycleScroller>
</template>
```

### Performance Tips

```javascript
// ✅ Computed au lieu de methods dans template
const fullName = computed(() => `${firstName.value} ${lastName.value}`)

// ❌ Éviter
<template>{{ getFullName() }}</template>

// ✅ Utiliser
<template>{{ fullName }}</template>

// ✅ v-show pour toggle fréquent
// ✅ v-if pour toggle rare

// ✅ Utiliser key unique dans v-for
<div v-for="item in items" :key="item.id">

// ✅ Éviter v-if et v-for ensemble
// ❌
<li v-for="user in users" v-if="user.isActive" :key="user.id">

// ✅
<li v-for="user in activeUsers" :key="user.id">

// ✅ Déstructurer uniquement ce dont on a besoin avec storeToRefs
const { count, name } = storeToRefs(store)
// Au lieu de
const store = useStore()
```

---

## Transitions

### Transition de base

```vue
<template>
  <button @click="show = !show">Toggle</button>
  
  <Transition name="fade">
    <p v-if="show">Hello</p>
  </Transition>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
```

### TransitionGroup

```vue
<template>
  <TransitionGroup name="list" tag="ul">
    <li v-for="item in items" :key="item.id">
      {{ item.name }}
    </li>
  </TransitionGroup>
</template>

<style>
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}
</style>
```

---

## Outils de développement

### Vue DevTools

```bash
# Chrome/Edge
https://chrome.google.com/webstore

# Firefox
https://addons.mozilla.org/firefox

# Standalone
npm install -g @vue/devtools
vue-devtools
```

### Vite plugins utiles

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'

export default defineConfig({
  plugins: [
    vue(),
    
    // Auto-import APIs
    AutoImport({
      imports: ['vue', 'vue-router', 'pinia'],
      dts: 'src/auto-imports.d.ts'
    }),
    
    // Auto-import composants
    Components({
      dts: 'src/components.d.ts'
    })
  ]
})
```

---

## Astuces et Best Practices

### 1. Préférer Composition API + script setup

```vue
<!-- ✅ Recommandé -->
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<!-- ❌ Plus verbeux -->
<script>
export default {
  setup() {
    const count = ref(0)
    return { count }
  }
}
</script>
```

### 2. Utiliser TypeScript

```vue
<script setup lang="ts">
// Type safety automatique
</script>
```

### 3. Extraire la logique en composables

```javascript
// Au lieu de dupliquer la logique
// Créer un composable réutilisable
import { useFetch } from '@/composables/useFetch'
```

### 4. Nommer les composants en PascalCase

```javascript
// ✅
import UserProfile from './UserProfile.vue'

// ❌
import userProfile from './user-profile.vue'
```

### 5. Utiliser defineModel pour v-model

```vue
<!-- Vue 3.4+ -->
<script setup>
const model = defineModel()
</script>
```

### 6. Pinia pour state management

```javascript
// ✅ Pinia (recommandé)
import { useTodoStore } from '@/stores/todo'

// ❌ Vuex (legacy)
```

### 7. Éviter les mutations directes de props

```javascript
// ❌
props.count++

// ✅
const emit = defineEmits(['update:count'])
emit('update:count', props.count + 1)
```

### 8. Utiliser computed pour les valeurs dérivées

```javascript
// ✅
const fullName = computed(() => `${firstName.value} ${lastName.value}`)

// ❌
const fullName = firstName.value + ' ' + lastName.value
```

---

## Ressources

- **Documentation officielle**: https://vuejs.org/
- **Vue Router**: https://router.vuejs.org/
- **Pinia**: https://pinia.vuejs.org/
- **Vite**: https://vitejs.dev/
- **Awesome Vue**: https://github.com/vuejs/awesome-vue
- **Vue Mastery**: https://www.vuemastery.com/
- **Vue School**: https://vueschool.io/

---
