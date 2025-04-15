<script setup lang="ts">
const toast = useToast();

function show() {
  toast.add({
    severity: "success",
    summary: "Success Message",
    detail: "Order submitted",
    life: 3000,
  })
}
</script>

<template>
    <NuxtRouteAnnouncer />
    <NuxtLayout>
      <Toast />
      <NuxtPage/>
    </NuxtLayout>
</template>
