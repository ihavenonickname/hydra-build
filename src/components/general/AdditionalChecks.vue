<script setup>
import { ref, watch, onMounted } from 'vue'

const props = defineProps({
  modelValue: {
    type: Object,
    required: true
  },
})

const emit = defineEmits(['update:modelValue'])

const refNullPassword = ref(false)
const refLoginAsPassword = ref(false)
const refReverseLoginAsPassword = ref(false)

watch(refNullPassword, () => validateAndEmit())
watch(refLoginAsPassword, () => validateAndEmit())
watch(refReverseLoginAsPassword, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
  const additionalChecksOptions = [];

  if (refNullPassword.value) {
    additionalChecksOptions.push('e');
  }

  if (refLoginAsPassword.value) {
    additionalChecksOptions.push('s');
  }

  if (refReverseLoginAsPassword.value) {
    additionalChecksOptions.push('r');
  }

  if (additionalChecksOptions.length > 0) {
    const additionalChecks = additionalChecksOptions.join('');
    emit('update:modelValue', { commandPart: `-e ${additionalChecks}`, error: null })
  } else {
    emit('update:modelValue', { commandPart: '', error: null })
  }
}
</script>

<template>
  <div class="field is-horizontal">
    <div class="field-label">
      <label class="label">Additional checks</label>
    </div>
    <div class="field-body">
      <div class="field is-narrow">
        <div class="control">
          <label class="radio">
            <input type="checkbox" v-model="refNullPassword">
            Null password
          </label>
          <br />
          <label class="radio">
            <input type="checkbox" v-model="refLoginAsPassword">
            Login as password
          </label>
          <br />
          <label class="radio">
            <input type="checkbox" v-model="refReverseLoginAsPassword">
            Reverse login as password
          </label>
        </div>
      </div>
    </div>
  </div>
</template>
