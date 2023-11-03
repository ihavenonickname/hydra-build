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

watch(refAnswer, () => validateAndEmit())

onMounted(() => validateAndEmit())

function validateAndEmit() {
    if (refAnswer.value === 'Yes') {
        emit('update:modelValue', { commandPart: '-u', error: null })
    } else {
        emit('update:modelValue', { commandPart: '', error: null })
    }
}
</script>

<template>
    <BasicBoolean v-model="refAnswer" title="Loop passwords" :options="['Yes', 'No']"
        help="By default Hydra checks all passwords for one login and then tries the next login. This option loops around the passwords, so the first password is tried on all logins, then the next password." />
</template>
