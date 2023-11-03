<script setup>
import { ref, watch, onMounted } from 'vue'
import BasicText from '@/components/form/basic-text.vue'

const props = defineProps({
  modelValue: {
    type: Object,
    required: true
  },
})

const emit = defineEmits(['update:modelValue'])

const refAnswer = ref('')

watch(refAnswer, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  const target = refAnswer.value.trim()

  if (target !== '') {
    emit('update:modelValue', { commandPart: target, error: null })
  } else {
    emit('update:modelValue', { commandPart: null, error: 'Inform a non-empty target' })
  }
}
</script>

<template>
  <BasicText v-model="refAnswer" title="Target"
    help="A target to attack, can be an IPv4 address, IPv6 address or DNS name." />
</template>
