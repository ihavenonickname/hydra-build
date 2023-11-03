<script setup>
import { ref, useSlots, watch } from 'vue'

const props = defineProps({
    title: {
        type: String,
        required: true
    },
    help: {
        type: String,
        required: true
    },
    options: {
        type: Array,
        required: true
    },
    modelValue: {
        type: String,
        required: true
    },
    modelValue: {
        type: String,
        required: true
    },
})

const emit = defineEmits(['update:modelValue'])

const slots = useSlots()

const uniqueName = Math.random()

const picked = ref(props.modelValue)

watch(picked, value => emit('update:modelValue', value))
</script>

<template>
    <div class="field is-horizontal">
        <div class="field-label">
            <label class="label">{{ title }}</label>
        </div>
        <div class="field-body">
            <div class="field is-narrow">
                <div class="control">
                    <label v-for="option in options" :key="option" class="radio">
                        <input type="radio" :name="uniqueName" :value="option" v-model="picked">
                        {{ option }}
                    </label>
                </div>
                <p class="help">{{ help }}</p>
                <div v-if="slots.default">
                    <slot :picked="picked"></slot>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>
