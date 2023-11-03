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

const refAnswer = ref('Single')
const refSinglePassword = ref('')
const refFilepath = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refSinglePassword, () => validateAndEmit())
watch(refFilepath, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Single') {
    const password = refSinglePassword.value.trim()

    if (password !== '') {
      emit('update:modelValue', { commandPart: `-p ${password}`, error: null })
    } else {
      emit('update:modelValue', { commandPart: null, error: 'Inform a non-empty password' })
    }
  } else {
    const filepath = refFilepath.value.trim()

    if (filepath !== '') {
      emit('update:modelValue', { commandPart: `-P ${filepath}`, error: null })
    } else {
      emit('update:modelValue', { commandPart: null, error: 'Inform a non-empty filepath for password' })
    }
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Password" :options="['Single', 'From file']"
    help="Try with this password or load several passwords from file.">
    <label v-if="slotProps.picked === 'Single'">
      Password
      <input type="text" v-model="refSinglePassword">
    </label>
    <label v-else>
      File
      <input type="text" v-model="refFilepath">
    </label>
  </BasicBoolean>
</template>
