<script setup>
import { defineAsyncComponent, ref, watch, onMounted } from 'vue'
import { computed } from '@vue/reactivity';

const components = [
  defineAsyncComponent(() => import('@/components/general/Target.vue')),
  defineAsyncComponent(() => import('@/components/general/RestoreSession.vue')),
  defineAsyncComponent(() => import('@/components/general/UseOldSsl.vue')),
  defineAsyncComponent(() => import('@/components/general/CustomPort.vue')),
  defineAsyncComponent(() => import('@/components/general/Login.vue')),
  defineAsyncComponent(() => import('@/components/general/Password.vue')),
  defineAsyncComponent(() => import('@/components/general/AdditionalChecks.vue')),
  defineAsyncComponent(() => import('@/components/general/LoopPasswords.vue')),
  defineAsyncComponent(() => import('@/components/general/ExitOnFirstFound.vue')),
  defineAsyncComponent(() => import('@/components/general/CustomParallelism.vue')),
  defineAsyncComponent(() => import('@/components/general/CustomTimeout.vue')),
  defineAsyncComponent(() => import('@/components/general/ConnectionDelay.vue')),
  defineAsyncComponent(() => import('@/components/general/AttemptDelay.vue')),
  defineAsyncComponent(() => import('@/components/general/PreferIpv6.vue')),
  defineAsyncComponent(() => import('@/components/general/Verbose.vue')),
  defineAsyncComponent(() => import('@/components/general/ShowAttempts.vue')),
  defineAsyncComponent(() => import('@/components/general/IgnoreRestore.vue')),
]

const emit = defineEmits(['update'])

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

watch(errors, () => emit('update', { errors: errors.value, commandParts: commandParts.value }))
watch(commandParts, () => emit('update', { errors: errors.value, commandParts: commandParts.value }))

function updateState(component, newState) {
  const i = components.indexOf(component)
  states.value[i] = newState
}

onMounted(() => emit('update', { errors: errors.value, commandParts: commandParts.value }))
</script>

<template>
  <div class="block">
    <section class="field-group">
      <h2 class="title is-4 has-text-centered">General options</h2>

      <template v-for="component in components" :key="component.name">
        <component :is="component" :modelValue="{}" @update:modelValue="updateState(component, $event)" />
      </template>
    </section>
  </div>
</template>
