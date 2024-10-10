<script setup lang="ts">
import type { Organizer } from '@/types'
import { ref } from 'vue'
import OrganizerService from '@/services/OrganizerService'
import { useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message'
import BaseInput from '@/components/BaseInput.vue'
import ImageUpload from '@/components/ImageUpload.vue'

const organizer = ref<Organizer>({
  id: 0,
  name: '',
  images: []
})
const router = useRouter()
const store = useMessageStore()
function saveOrganizer() {
  OrganizerService.saveOrganizer(organizer.value)
    .then((response) => {
      router.push({ name: 'organizer-detail-view', params: { id: response.data.id } })
      store.updateMessage(
        'You are successfully add a new organizer for id number ' + response.data.id
      )
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
      <h3>Name your organizer</h3>
      <BaseInput v-model="organizer.name" type="text" label="Name" />
      <h3>The image of the Organizer</h3>
      <ImageUpload v-model="organizer.images" />
      <button class="button" type="submit">Submit</button>
    </form>

    <pre>{{ organizer }}</pre>
  </div>
</template>
