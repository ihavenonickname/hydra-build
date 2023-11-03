<script setup>
import { ref, watch, onMounted } from 'vue'
import BasicBoolean from './form/basic-boolean.vue'

const props = defineProps({
  modelValue: {
    type: Object,
    required: true
  },
})

const emit = defineEmits(['update:modelValue'])

const refAnswer = ref('No')
const refSeconds = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refSeconds, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Yes') {
    const seconds = parseInt(refSeconds.value);

    if (isNaN(seconds) || seconds < 1) {
      emit('update:modelValue', { commandPart: null, error: 'Inform a valid number of seconds' })
    } else {
      emit('update:modelValue', { commandPart: `-w ${seconds}`, error: null })
    }
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Custom timeout" :options="['Yes', 'No']"
    help="Defines the max wait time in seconds for responses.">
    <label v-if="slotProps.picked === 'Yes'">
      Seconds
      <input type="text" v-model="refSeconds">
    </label>
  </BasicBoolean>
</template>
