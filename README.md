grammy-vue
---

Simple utility hook to call the Telegram API using [grammy](https://grammy.dev) and track the request status with Vue.js refs

## Usage

```html
<!-- App.vue -->
<script lang="ts" setup>
import { ref } from 'vue';
import { useTelegramApi } from 'grammy-vue';

const token = ref('')
const { useApiMethod } = useTelegramApi(token)
const { refresh: getMe, state, data: botInfo, error } = useApiMethod('getMe')
</script>

<template>
  <input type="text" v-model="token" />
  <button @click="getMe">GetMe</button>
  <div v-if="state === 'idle'">Type the token and click the button</div>
  <div v-if="state === 'loading'">Loading bot info...</div>
  <div v-if="state === 'error'">Grammy error: {{ error.message }}</div>
  <div v-if="state === 'success'">Loaded info for bot {{ botInfo.username }}</div>
</template>
```

### Resetting status after a set amount of time

The `useApiMethod` hook takes an optional second parameter, which is the number of milliseconds to wait before setting the state back to `idle`. If you don't specify this parameter, the state will not be automatically reset.

```html
<!-- App.vue -->
<script lang="ts" setup>
import { ref } from 'vue';
import { useTelegramApi } from 'grammy-vue';

const token = ref('')
const url = ref('')
const { useApiMethod } = useTelegramApi(token)
const { refresh: setWebhook, state, error } = useApiMethod('setWebhook', 1000) // state will be reset after 1 second
</script>

<template>
  <input type="text" v-model="token" />
  <input type="text" v-model="url" />
  <button @click="getMe(url)">SetWebhook</button>
  <div v-if="state === 'idle'">Type the token and the url, then click the button</div>
  <div v-if="state === 'loading'">Setting webhook...</div>
  <div v-if="state === 'error'">Grammy error: {{ error.message }}</div>
  <div v-if="state === 'success'">Webhook was set!</div>
</template>
```