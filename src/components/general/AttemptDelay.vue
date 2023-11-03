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
const refSeconds = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refSeconds, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Yes') {
    const seconds = parseInt(refSeconds.value);

    if (isNaN(seconds) || seconds < 1) {
      emit('update:modelValue', { commandPart: null, error: 'Inform a valid number of seconds for attempt delay' })
    } else {
      emit('update:modelValue', { commandPart: `-c ${seconds}`, error: null })
    }
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Attempt delay" :options="['Yes', 'No']"
    help="The wait time in seconds per login attempt over all threads. This usually only makes sense if a low task number is used.">
    <label v-if="slotProps.picked === 'Yes'">
      Seconds
      <input type="text" v-model="refSeconds">
    </label>
  </BasicBoolean>
</template>
