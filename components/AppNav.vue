<script setup lang="ts">
defineProps<{
  closeable?: boolean
  onClose?: () => void
}>()

const items = [
  {
    label: "Cadastro Único",
    items: [{ label: "Test" }]
  },
  {
    label: "PAIF",
    items: [{ label: "Test" }]
  },
  {
    label: "SCFV",
    items: [
      {
        label: "Usuário - NÃO IMPLEMENTADO",
        icon: "pi pi-user",
        items: [
          { label: "Novo usuário", icon: "pi pi-plus" },
          { label: "Ver usuário", icon: "pi pi-search" }
        ]
      },
      {
        label: "Grupos",
        icon: "pi pi-users",
        items: [
          {
            label: "Novo grupo",
            icon: "pi pi-plus",
            route: "/scfv/grupos/novo_grupo"
          },
          {
            label: "Ver grupos",
            icon: "pi pi-search",
            route: "/scfv/grupos/ver_grupos"
          },
          {
            label: "Vincular usuários",
            icon: "pi pi-user-plus",
            route: "/scfv/grupos/vincular_usuario"
          },
          {
            label: "Lista de presença",
            icon: "pi pi-clipboard",
            route: "/scfv/grupos/lista_presenca"
          }
        ]
      },
      {
        label: "Métricas - NÃO IMPLEMENTADO",
        icon: "pi pi-chart-bar"
      }
    ]
  },
  {
    label: "PCF",
    items: [{ label: "Test" }]
  },
  {
    label: "Sair",
    icon: "pi pi-sign-out",
    to: "/login"
  }
]
</script>

<template>
  <div class="card flex flex-col justify-start w-full md:w-64 p-4 md:p-0 space-y-4">
    <!-- Botão de fechar para mobile -->
    <div v-if="closeable" class="flex justify-end px-2">
      <Button
        icon="pi pi-times"
        class="p-button-text"
        @click="onClose?.()"
        aria-label="Fechar menu"
      />
    </div>

    <!-- Menu principal -->
    <PanelMenu :model="items" class="w-full">
      <template #item="{ item }">
        <router-link
          v-if="item.route"
          v-slot="{ href, navigate }"
          :to="item.route"
          custom
        >
          <a
            v-ripple
            class="flex items-center cursor-pointer text-surface-700 dark:text-surface-0 px-4 py-2"
            :href="href"
            @click="navigate"
          >
            <span :class="item.icon" />
            <span class="ml-2">{{ item.label }}</span>
          </a>
        </router-link>
        <a
          v-else
          v-ripple
          class="flex items-center cursor-pointer text-surface-700 dark:text-surface-0 px-4 py-2"
          :href="item.url"
          :target="item.target"
        >
          <span :class="item.icon" />
          <span class="ml-2">{{ item.label }}</span>
          <span
            v-if="item.items"
            class="pi pi-angle-down text-primary ml-auto"
          />
        </a>
      </template>
    </PanelMenu>
  </div>
</template>