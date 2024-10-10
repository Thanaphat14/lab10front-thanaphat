<script setup lang="ts">
import { toRefs, defineProps } from 'vue'
import { type Organizer } from '@/types'

const props = defineProps<{
  organizer: Organizer
  id: String
}>()
const { organizer } = toRefs(props)
</script>
<template>
  <p><b>ID:</b> {{ organizer.id }}</p>
  <p><b>Name:</b> {{ organizer.name }}</p>
  <p><b>Address:</b>{{ organizer.address }}</p>
  <div class="flex flex-row flex-wrap justify-center">
    <img v-for="image in event.images" :key="image" :src="image" alt="events image"
      class="border-solid border-gray-200 border-2 rounded p-1 m-1 w-40
      hover:shadow-lg" />
  </div>
</template>
