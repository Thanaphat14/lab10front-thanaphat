<script setup lang="ts">
import Uploader from 'vue-media-upload'
import { ref } from 'vue'

interface Props {
  modelValue?: string
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: ''
})

const emit = defineEmits(['update:modelValue'])

const convertStringToMedia = (str: string): any => {
  return {
    name: str
  }
}

const convertMediaToString = (media: any): string => {
  return media[0].name
}

const media = ref(props.modelValue ? [convertStringToMedia(props.modelValue)] : [])
const uploadUrl = ref(import.meta.env.VITE_UPLOAD_URL)

const onChanged = (files: any) => {
  if (files.length > 0) {
    media.value = [files[0]] // Only keep the first file
    emit('update:modelValue', convertMediaToString(files))
  }
}
</script>

<template>
  <Uploader 
    :server="uploadUrl" 
    @change="onChanged" 
    :media="media" 
    :max="1"  
  />
</template>
