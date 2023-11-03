<script setup>
import { ref, watch, onMounted } from 'vue'
import BasicBoolean from '@/components/form/basic-boolean.vue'

const props = defineProps({
  modelValue: {
    type: Object,
    required: true
  },
})

const emit = defineEmits(['update:modelValue'])

const refAnswer = ref('No')

watch(refAnswer, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Yes') {
    emit('update:modelValue', { commandPart: '-O', error: null })
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-model="refAnswer" title="Use old SSL" :options="['Yes', 'No']"
    help="Restore a previously aborted session. Requires a hydra.restore file was written." />
</template>
