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
    emit('update:modelValue', { commandPart: '-V', error: null })
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <BasicBoolean v-model="refAnswer" title="Show attempts" :options="['Yes', 'No']"
    help="Show login+pass combination for each attempt." />
</template>
