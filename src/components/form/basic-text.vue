<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  title: {
    type: String,
    required: true
  },
  help: {
    type: String,
    required: true
  },
  modelValue: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['update:modelValue'])

const picked = ref(props.modelValue)

watch(picked, newValue => emit('update:modelValue', newValue))
</script>

<template>
  <div class="field is-horizontal">
    <div class="field-label">
      <label class="label">{{ title }}</label>
    </div>
    <div class="field-body">
      <div class="field is-narrow">
        <div class="control">
          <input v-model="picked" class="input" type="text">
        </div>
        <p class="help">{{ help }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
