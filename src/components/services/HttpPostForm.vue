<script setup>
import { ref, watch, onMounted } from 'vue'

const emit = defineEmits(['update'])

const refEndpoint = ref('')
const refFormData = ref('')
const refCondition = ref('')

watch(refEndpoint, updateAndEmit)
watch(refFormData, updateAndEmit)
watch(refCondition, updateAndEmit)

onMounted(() => updateAndEmit())

function updateAndEmit() {
  const errors = []

  const endpoint = refEndpoint.value.trim().replace(':', '\\:')

  if (endpoint === '') {
    errors.push('Inform a non-empty endpoint')
  }

  const formData = refFormData.value.trim().replace(':', '\\:')

  if (formData === '') {
    errors.push('Inform a non-empty form data')
  }

  const condition = refCondition.value.trim().replace(':', '\\:')

  if (condition === '') {
    errors.push('Inform a non-empty condition')
  }

  console.log(errors)

  if (errors.length === 0) {
    emit('update', { errors, commandPart: `-m "${endpoint}:${formData}:F=${condition}"` })
  } else {
    emit('update', { errors, commandPart: null })
  }
}
</script>

<template>
  <div>

  </div>

  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label"></label>
    </div>

    <div class="field-body">
      <div class="field">
        <div class="control">
          <article class="message is-primary">
            <div class="message-body">
              <span class="has-text-weight-semibold">Note</span>: Colons are escaped automatically
            </div>
          </article>
        </div>
      </div>
    </div>
  </div>

  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Endpoint</label>
    </div>

    <div class="field-body">
      <div class="field">
        <div class="control">
          <input v-model="refEndpoint" class="input" type="text">
        </div>
        <p class="help">
          The login endpoint. E.g.: /login.php
        </p>
      </div>
    </div>
  </div>

  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Form</label>
    </div>

    <div class="field-body">
      <div class="field">
        <div class="control">
          <input v-model="refFormData" class="input" type="text">
        </div>
        <p class="help">
          The raw form data to be sent to the login endpoint. Put ^USER^ and ^PASS^ where Hydra
          should replace with actial login and password values. E.g.: username=^USER^&password=^PASS^
        </p>
      </div>
    </div>
  </div>

  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Failed condition</label>
    </div>

    <div class="field-body">
      <div class="field">
        <div class="control">
          <input v-model="refCondition" class="input" type="text">
        </div>
        <p class="help">
          A string the appears in the page when the login fails. If this piece of text is NOT
          present in the response body, Hydra understands that the login was successful.
        </p>
      </div>
    </div>
  </div>
</template>
