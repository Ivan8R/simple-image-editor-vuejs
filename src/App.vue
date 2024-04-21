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
'https://images.unsplash.com/photo-1481349518771-20055b2a7b24?q=80&w=1539&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
  const toggleModal = () => {
    modalActive.value = !modalActive.value
  }

</script>

<style scoped>
</style>
