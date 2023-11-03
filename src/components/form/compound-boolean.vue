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
        type: Boolean,
        required: true
    }
})

const emit = defineEmits(['update:modelValue'])

const uniqueName = Math.random()

const picked = ref(props.modelValue ? 'yes' : 'no')

watch(picked, newValue => emit('update:modelValue', newValue === 'yes'))
</script>

<template>
    <div class="field is-horizontal">
        <div class="field-label">
            <label class="label">{{ title }}</label>
        </div>
        <div class="field-body">
            <div class="field is-narrow">
                <div class="control">
                    <label class="radio">
                        <input type="radio" :name="uniqueName" value="yes" v-model="picked">
                        Yes
                    </label>
                    <label class="radio">
                        <input type="radio" :name="uniqueName" value="no" v-model="picked">
                        No
                    </label>
                </div>
                <p class="help">{{ help }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>
