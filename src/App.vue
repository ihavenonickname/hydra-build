<script setup>
import { defineAsyncComponent, ref } from 'vue'
import { computed } from '@vue/reactivity';

const components = [
  defineAsyncComponent(() => import('./components/Target.vue')),
  defineAsyncComponent(() => import('./components/RestoreSession.vue')),
  defineAsyncComponent(() => import('./components/UseOldSsl.vue')),
  defineAsyncComponent(() => import('./components/CustomPort.vue')),
  defineAsyncComponent(() => import('./components/Login.vue')),
  defineAsyncComponent(() => import('./components/Password.vue')),
  defineAsyncComponent(() => import('./components/AdditionalChecks.vue')),
  defineAsyncComponent(() => import('./components/LoopPasswords.vue')),
  defineAsyncComponent(() => import('./components/ExitOnFirstFound.vue')),
  defineAsyncComponent(() => import('./components/CustomParallelism.vue')),
  defineAsyncComponent(() => import('./components/CustomTimeout.vue')),
  defineAsyncComponent(() => import('./components/ConnectionDelay.vue')),
  defineAsyncComponent(() => import('./components/AttemptDelay.vue')),
  defineAsyncComponent(() => import('./components/PreferIpv6.vue')),
  defineAsyncComponent(() => import('./components/Verbose.vue')),
  defineAsyncComponent(() => import('./components/ShowAttempts.vue')),
  defineAsyncComponent(() => import('./components/IgnoreRestore.vue')),
]

const states = ref([])

states.value = components.map(() => ({}))

const errors = computed(() => states.value
  .map(({ error }) => error)
  .filter(error => error)
)

const commandParts = computed(() => states.value
  .map(({ commandPart }) => commandPart)
  .filter(commandPart => commandPart)
)

const command = computed(() => ['hydra', ...commandParts.value].join(' '))

const commandIsValid = computed(() => errors.value.length === 0)

function copyToClipboard() {
  navigator.clipboard.writeText(command.value);
}

function updateState(component, newState) {
  const i = components.indexOf(component)
  states.value[i] = newState
}
</script>

<template>
  <main>
    <div class="has-text-centered block">
      <h1 class="title is-1">Hydra Builder</h1>
      <p class="subtitle">A command builder for the Hydra logon cracker tool</p>
    </div>

    <section class="field-group block">
      <h2 class="title is-4 has-text-centered">General options</h2>

      <template v-for="component in components" :key="component.name">
        <component :is="component" :modelValue="{}" @update:modelValue="updateState(component, $event)" />
      </template>
    </section>

    <section class="field-group block">
      <h2 class="title is-4 has-text-centered">Hydra command</h2>

      <pre v-if="commandIsValid" class="has-text-centered"><code>{{ command }}</code></pre>
      <p v-else v-for="error in errors" class="has-text-centered is-size-4">{{ error }}</p>

      <div v-if="commandIsValid" class="has-text-centered">
        <button class="button is-primary" @click="copyToClipboard">
          Copy to clipboard
        </button>
      </div>
    </section>

    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          I wonder what I should write in the footer of this page
        </p>
      </div>
    </footer>
  </main>
</template>

<style scoped>
pre,
code {
  white-space: pre-line;
  font-size: larger;
  background: none;
  padding: 0;
}

.field-group {
  border: 1px solid lightgrey;
  border-radius: 10px;
  padding: 10px 0;
  margin: 5px;
}

.radio {
  user-select: none;
}

.fields-container {
  margin: 5px;
}

@media (min-width: 960px) {
  .field-group {
    margin: 0 auto;
    max-width: 960px;
  }

  .fields-container {
    margin: 15px;
  }
}
</style>
