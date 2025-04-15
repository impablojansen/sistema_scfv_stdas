<script setup lang="ts">
import { ref } from 'vue'

const menuAberto = ref(false)

function toggleMenu() {
  menuAberto.value = !menuAberto.value
}

function fecharMenu() {
  menuAberto.value = false
}
</script>

<template>
  <div class="flex min-h-screen relative">
    <!-- Menu lateral fixo no desktop -->
    <aside class="hidden md:block w-80 p-4 bg-white shadow z-10">
      <AppNav />
    </aside>

    <!-- Botão hamburguer no mobile -->
    <div class="md:hidden fixed top-4 left-4 z-50">
      <Button icon="pi pi-bars" @click="toggleMenu" rounded class="shadow" />
    </div>

    <!-- Overlay escuro ao fundo quando o menu está aberto -->
    <div
      v-if="menuAberto"
      class="fixed inset-0 bg-opacity-50 z-40 md:hidden"
      @click="fecharMenu"
    ></div>

    <!-- Menu lateral deslizante no mobile -->
    <aside
      class="fixed top-0 left-0 h-full w-72 bg-white shadow transform transition-transform duration-300 ease-in-out z-50 md:hidden"
      :class="menuAberto ? 'translate-x-0' : '-translate-x-full'"
    >
      <AppNav />
    </aside>

    <!-- Conteúdo principal -->
    <main class="flex-1 p-4 bg-gray-50 pt-16 md:pt-4">
      <NuxtPage />
    </main>
  </div>
</template>