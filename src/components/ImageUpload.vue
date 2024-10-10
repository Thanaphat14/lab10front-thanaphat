<script setup lang="ts">
import Uploader from 'vue-media-upload'
import { ref } from 'vue'

interface Props {
  modelValue?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: () => []
})

const emit = defineEmits(['update:modelValue'])

const media = ref(props.modelValue.length > 0 ? [{ name: props.modelValue[0] }] : [])
const uploadUrl = ref(import.meta.env.VITE_UPLOAD_URL)

const onChanged = (files: any) => {
  if (files.length > 0) {
    media.value = [files[0]]
    emit('update:modelValue', [files[0].name])
  } else {
    media.value = []
    emit('update:modelValue', [])
  }
}
</script>

<template>
  <Uploader :server="uploadUrl" @change="onChanged" :media="media"></Uploader>
</template>
