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
const refSingleLogin = ref('')
const refFilepath = ref('')

watch(refAnswer, () => validateAndEmit())
watch(refSingleLogin, () => validateAndEmit())
watch(refFilepath, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  if (refAnswer.value === 'Single') {
    const login = refSingleLogin.value.trim()

    if (login !== '') {
      emit('update:modelValue', { commandPart: `-l ${login}`, error: null })
    } else {
      emit('update:modelValue', { commandPart: null, error: 'Inform a non-empty login' })
    }
  } else {
    const filepath = refFilepath.value.trim()

    if (filepath !== '') {
      emit('update:modelValue', { commandPart: `-L ${filepath}`, error: null })
    } else {
      emit('update:modelValue', { commandPart: null, error: 'Inform a non-empty filepath for login' })
    }
  }
}
</script>

<template>
  <BasicBoolean v-slot="slotProps" v-model="refAnswer" title="Login" :options="['Single', 'From file']"
    help="Brute force a single login or load several logins from file.">
    <label v-if="slotProps.picked === 'Single'">
      Login
      <input type="text" v-model="refSingleLogin">
    </label>
    <label v-else>
      File
      <input type="text" v-model="refFilepath">
    </label>
  </BasicBoolean>
</template>
