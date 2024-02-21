<template>
  <Modal @close="toggleModal" :modalActive="modalActive" :image="image" />
  <button
    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded m-2"
    @click="toggleModal"
  >
    Button
  </button>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Modal from '@/components/modalView.vue'

  const modalActive = ref(false)
  const image =
    'https://global.discourse-cdn.com/business7/uploads/glideapps/original/2X/4/4936e0f28f79c8a2aa95582741091356d0ed61d8.jpeg'
  const toggleModal = () => {
    modalActive.value = !modalActive.value
  }
</script>

<style scoped>
</style>
