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
const refTaskCount = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refTaskCount, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Yes') {
    const tasks = parseInt(refTaskCount.value);

    if (isNaN(tasks) || tasks < 1) {
      emit('update:modelValue', { commandPart: null, error: 'Inform a valid number of tasks' })
    } else {
      emit('update:modelValue', { commandPart: `-t ${tasks}`, error: null })
    }
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Custom parallelism" :options="['Yes', 'No']"
    help="Run this number of connects in parallel.">
    <label v-if="slotProps.picked === 'Yes'">
      Tasks
      <input type="text" v-model="refTaskCount">
    </label>
  </BasicBoolean>
</template>
