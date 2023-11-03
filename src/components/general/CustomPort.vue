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
const refCustomPort = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refCustomPort, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Yes') {
    const port = parseInt(refCustomPort.value);

    if (isNaN(port) || port < 1 || port >= Math.pow(2, 16)) {
      emit('update:modelValue', { commandPart: null, error: 'Inform a valid port' })
    } else {
      emit('update:modelValue', { commandPart: `-s ${port}`, error: null })
    }
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Custom port" :options="['Yes', 'No']"
    help="If the service is on a different default port, define it here.">
    <label v-if="slotProps.picked === 'Yes'">
      Port
      <input type="text" v-model="refCustomPort">
    </label>
  </BasicBoolean>
</template>
