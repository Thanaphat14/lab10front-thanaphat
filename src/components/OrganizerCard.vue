<script setup lang="ts">
import { type Organizer } from '@/types'
defineProps<{
  organizer: Organizer
}>()
// const event = ref({
//   id: 5928101,
//   category: 'animal welfare',
//   title: 'Cat Adoption Day',
//   description: 'Find your new feline friend at this event.',
//   location: 'Meow Town',
//   date: 'January 28, 2022',
//   time: '12:00',
//   petsAllowed: true,
//   organizer: 'Kat Laydee'
// })
</script>

<template>
  <RouterLink
    class="organizer-link"
    :to="{ name: 'organizer-detail-view', params: { id: organizer.id } }"
  >
    <div
      class="cursor-pointer border border-gray-600 p-[20px] w-[250px] mb-[18px] hover:scale-101 hover:shadow-sp"
    >
      <h2>Organizer ID: {{ organizer.id }}</h2>
      <span>Name: {{ organizer.name }}</span>
    </div>
  </RouterLink>
</template>

<style scoped>
.organizer-link {
  text-decoration: none;
  color: #2c3e50;
}
</style>
