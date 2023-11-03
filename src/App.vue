<script setup>
import { ref } from 'vue'
import { computed } from '@vue/reactivity';
import General from '@/components/General.vue'

const errorsGeneral = ref([])
const commandPartsGeneral = ref([])

const errors = computed(() => [...errorsGeneral.value])
const commandParts = computed(() => [...commandPartsGeneral.value])
const command = computed(() => ['hydra', ...commandParts.value].join(' '))
const commandIsValid = computed(() => errors.value.length === 0)

function copyToClipboard() {
  navigator.clipboard.writeText(command.value);
}

function handleUpdateGeneral(ev) {
  errorsGeneral.value = ev.errors
  commandPartsGeneral.value = ev.commandParts
}
</script>

<template>
  <main>
    <section class="hero has-text-centered">
      <div class="hero-body">
        <p class="title">
          Hydra Builder
        </p>
        <p class="subtitle">
          A command builder for the Hydra logon cracker tool
        </p>
      </div>
    </section>

    <General @update="handleUpdateGeneral" />


    <div class="block">
      <section class="field-group">
        <h2 class="title is-4 has-text-centered">Hydra command</h2>

        <pre v-if="commandIsValid" class="has-text-centered"><code>{{ command }}</code></pre>
        <p v-else v-for="error in errors" class="has-text-centered is-size-4">{{ error }}</p>

        <div v-if="commandIsValid" class="has-text-centered">
          <button class="button is-primary" @click="copyToClipboard">
            Copy to clipboard
          </button>
        </div>
      </section>
    </div>

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
</style>
