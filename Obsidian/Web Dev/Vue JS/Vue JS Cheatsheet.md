Composition API

Overview

The Composition API in Vue.js provides a flexible and modular way to organize code within components. It is designed to make codebases easier to maintain, especially in large applications.

1. **Setup**

📦 **To use Vue 3 with the Composition API, include the Vue CDN**:

Auto

```
<script src="https://unpkg.com/vue@3"></script>

```

🖥️ **Or, install Vue 3 via npm**:

Auto

```
npm install vue@next

```

1. **Core Composition API Functions**

a. **Reactive State**

🌟 reactive: Creates a reactive object.

Auto

```
import { reactive } from 'vue';

const state = reactive({ count: 0 });

```

🌟 ref: Creates a reactive reference for primitive values.

Auto

```
import { ref } from 'vue';

const count = ref(0);

```

b. **Computed Properties**

🔢 computed: Derives reactive values based on other state.

Auto

```
import { computed } from 'vue';

const count = ref(0);
const doubleCount = computed(() => count.value * 2);

```

c. **Lifecycle Hooks**

Use lifecycle hooks with their respective functions:

⏳ onMounted: Executes when the component is mounted.

🔄 onUpdated: Executes when the component updates.

❌ onUnmounted: Executes when the component is destroyed.

Auto

```
import { onMounted, onUpdated, onUnmounted } from 'vue';

onMounted(() => console.log('Mounted!'));
onUpdated(() => console.log('Updated!'));
onUnmounted(() => console.log('Unmounted!'));

```

d. **Watchers**

👀 watch: Responds to changes in reactive data.

Auto

```
import { ref, watch } from 'vue';

const count = ref(0);

watch(count, (newValue, oldValue) => {
  console.log(`Count changed from ${oldValue} to ${newValue}`);
});

```

👀 watchEffect: Automatically tracks reactive dependencies.

Auto

```
import { ref, watchEffect } from 'vue';

const count = ref(0);

watchEffect(() => {
  console.log(`Count is: ${count.value}`);
});

```

e. **Props and Emits**

📥 **Using Props**: Define props in defineProps for components.

Auto

```
<script setup>
defineProps(['title']);
</script>

<template>
  <h1>{{ title }}</h1>
</template>

```

📤 **Using Emits**: Define and emit events using defineEmits.

Auto

```
<script setup>
const emit = defineEmits(['submit']);

const handleSubmit = () => {
  emit('submit', 'Submitted!');
};
</script>

<template>
  <button @click="handleSubmit">Submit</button>
</template>

```

1. **Template Syntax with Composition API**

a. **Reactive Properties in Templates**

Use ref and reactive variables directly in templates:

Auto

```
<script setup>
import { ref } from 'vue';

const message = ref('Hello, Vue!');
</script>

<template>
  <p>{{ message }}</p>
</template>

```

b. **Event Handling**

Bind events with methods or inline functions:

Auto

```
<script setup>
import { ref } from 'vue';

const count = ref(0);
const increment = () => count.value++;
</script>

<template>
  <button @click="increment">Clicked {{ count }} times</button>
</template>

```

c. **Conditional Rendering**

Use v-if, v-else, and v-else-if for conditional rendering:

Auto

```
<script setup>
import { ref } from 'vue';

const isVisible = ref(true);
</script>

<template>
  <div v-if="isVisible">Visible</div>
  <div v-else>Hidden</div>
</template>

```

d. **List Rendering**

Use v-for to iterate over items:

Auto

```
<script setup>
import { ref } from 'vue';

const items = ref(['Apple', 'Banana', 'Cherry']);
</script>

<template>
  <ul>
    <li v-for="(item, index) in items" :key="index">{{ item }}</li>
  </ul>
</template>

```

1. **Modularizing with** setup

Encapsulate logic into composable functions:

Auto

```
// useCounter.js
import { ref } from 'vue';

export function useCounter() {
  const count = ref(0);
  const increment = () => count.value++;

  return { count, increment };
}

```

Usage in Component

Auto

```
<script setup>
import { useCounter } from './useCounter';

const { count, increment } = useCounter();
</script>

<template>
  <button @click="increment">Count: {{ count }}</button>
</template>

```

1. **Advanced Features**

a. **Provide/Inject**

Share data between parent and child components.

🤝 **Provide in Parent**:

Auto

```
import { provide } from 'vue';

provide('key', 'value');

```

🤝 **Inject in Child**:

Auto

```
import { inject } from 'vue';

const value = inject('key');

```

b. **Dynamic Components**

Render components dynamically using <component>:

Auto

```
<script setup>
import { ref } from 'vue';
import ComponentA from './ComponentA.vue';
import ComponentB from './ComponentB.vue';

const currentComponent = ref('ComponentA');
</script>

<template>
  <component :is="currentComponent" />
</template>

```

c. **Teleport**

Render a component outside of its parent DOM tree:

Auto

```
<template>
  <teleport to="#modal">
    <div class="modal">I am a modal</div>
  </teleport>
</template>

```

1. **Common Gotchas**

⚠️ Always use .value to access ref values.

⚠️ Ensure reactive objects are only used inside reactive or ref.

⚠️ Avoid using reactive with primitive values; use ref instead.

⚠️ Watchers are asynchronous; use flush: 'sync' if needed.

1. **Tips and Tricks**

💡 Use script setup for simpler and more concise components. 💡 Modularize logic with composable functions to keep code DRY. 💡 Leverage watchEffect for automatic dependency tracking. 💡 Use teleport for modals or overlays that need to break out of the parent DOM tree. 💡 Keep components small and focused for better readability and maintainability.

1. **Resources**

📖 Official Vue.js Documentation

📖 Vue.js Composition API Guide

🛠️ Vue.js Playground

Upload or embed a file