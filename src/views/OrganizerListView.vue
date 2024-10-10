<script setup lang="ts">
import OrganizerCard from '@/components/OrganizerCard.vue'
import { type Organizer } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import OrganizerService from '@/services/OrganizerService'
import { useRouter } from 'vue-router'

const router = useRouter()
const organizers = ref<Organizer[] | null>(null)
const totalOrganizers = ref(0)
const hasNexPage = computed(() => {
  const totalPages = Math.ceil(totalOrganizers.value / 3)
  return page.value < totalPages
})
const props = defineProps({
  page: {
    type: Number,
    required: true
  }
})
const page = computed(() => props.page)
onMounted(() => {
  watchEffect(() => {
    OrganizerService.getOrganizers()
      .then((response) => {
        organizers.value = response.data
        totalOrganizers.value = response.headers['x-total-count']
      })
      .catch(() => {
        router.push({ name: 'network-error-view' })
      })
  })
})
</script>

<template>
  <h1>Organizer</h1>
  <!-- new element -->
  <main class="flex flex-col items-center">
    <OrganizerCard v-for="organizer in organizers" :key="organizer.id" :organizer="organizer" />
    <div class="pagination">
      <RouterLink
        id="page-prev"
        :to="{ name: 'organizer-list-view', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Prev Page</RouterLink
      >

      <RouterLink
        id="page-next"
        :to="{ name: 'organizer-list-view', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNexPage"
        >Next Page &#62;</RouterLink
      >
    </div>
  </main>
</template>

<style scoped>
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>
