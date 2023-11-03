<script setup>
import { ref } from 'vue'

import Target from './components/Target.vue'
import RestoreSession from './components/RestoreSession.vue'
import UseOldSsl from './components/UseOldSsl.vue'
import CustomPort from './components/CustomPort.vue'
import Login from './components/Login.vue'
import Password from './components/Password.vue'
import AdditionalChecks from './components/AdditionalChecks.vue'

import { computed } from '@vue/reactivity';

const refTargetState = ref({})
const refRestoreSessionState = ref({})
const refUseOldSslState = ref({})
const refCustomPortState = ref({})
const refLoginState = ref({})
const refPasswordState = ref({})
const refAdditionalChecksState = ref({})

const states = computed(() => [
  refTargetState.value,
  refRestoreSessionState.value,
  refUseOldSslState.value,
  refCustomPortState.value,
  refLoginState.value,
  refPasswordState.value,
  refAdditionalChecksState.value,
])

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
</script>

<template>
  <main>
    <div class="has-text-centered block">
      <h1 class="title is-1">Hydra Builder</h1>
      <p class="subtitle">A command builder for the Hydra logon cracker tool</p>
    </div>

    <section class="field-group block">
      <h2 class="title is-4 has-text-centered">General options</h2>

      <Target v-model="refTargetState" />
      <RestoreSession v-model="refRestoreSessionState" />
      <UseOldSsl v-model="refUseOldSslState" />
      <CustomPort v-model="refCustomPortState" />
      <Login v-model="refLoginState" />
      <Password v-model="refPasswordState" />
      <AdditionalChecks v-model="refAdditionalChecksState" />
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
