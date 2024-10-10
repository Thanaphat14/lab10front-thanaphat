<script setup lang="ts">
import type { Organizer } from '@/types'
import { ref } from 'vue'
import OrganizerService from '@/services/OrganizerService'
import { useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message'
import ImageUpload from '@/components/ImageUpload.vue'

const organizer = ref<Organizer>({
  id: 0,
  name: '',
  images: []

})
const router = useRouter()
const store = useMessageStore()
function saveOrganizer() {
  OrganizerService.saveEvent(organizer.value)
    .then((response) => {
      router.push({ name: 'organizer-detail-view', params: { id: response.data.id } })
      store.updateMessage('You are successfully add a new event for id number ' + response.data.id)
      setTimeout(() => {
        store.resetMessage()
      }, 3000)
    })
    .catch(() => {
      router.push({ name: 'network-error-view' })
    })
}
</script>

<template>
  <div>
    <h1>Create an organizer</h1>
    <form @submit.prevent="saveOrganizer">
      <h3>Name & describe your event</h3>
      <label>Name</label>
      <input v-model="organizer.name" type="text" placeholder="Name" class="field" />
      <label>Address</label>
      <input v-model="organizer.address" type="text" placeholder="Address" class="field" />
      <h3>The image of the Event</h3>
      <ImageUpload v-model="organizer.images" />
      <button class="button" type="submit">Submit</button>
    </form>

    <pre>{{ organizer }}</pre>
  </div>
</template>
