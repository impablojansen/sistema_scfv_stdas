<script setup lang="ts">
const termoBusca = ref('')
const filtroCpf = ref('')
const usuarios = ref([
  { id: 1, nome: 'Ana Beatriz', cpf: '11111111111' },
  { id: 2, nome: 'Ana Paula', cpf: '22222222222' },
  { id: 3, nome: 'João Carlos', cpf: '00000000000' },
])

const grupos = ref([
  { id: 1, nome: 'Grupo Adolescentes 12-17' },
  { id: 2, nome: 'Grupo Idosos 60+' },
])

// resultado filtrado
const usuariosFiltrados = computed(() => {
  return usuarios.value.filter(u =>
    (!termoBusca.value || u.nome.toLowerCase().includes(termoBusca.value.toLowerCase())) &&
    (!filtroCpf.value || u.cpf.includes(filtroCpf.value))
  )
})

// vinculo temporário
const selecaoGrupo = ref({}) // exemplo: { 1: 2 } => usuário 1 escolheu grupo 2

function vincular(usuarioId: number) {
  const grupoId = selecaoGrupo.value[usuarioId]
  if (!grupoId) {
    alert('Selecione um grupo primeiro.')
    return
  }

  // CHAMADA API
  console.log(`Vincular usuário ${usuarioId} ao grupo ${grupoId}`)
  alert('Usuário vinculado com sucesso!')
}
</script>

<template>
    <div class="p-4">
      <h2 class="text-2xl font-semibold mb-4">Vincular usuário a grupo</h2>
  
      <!-- Filtros de busca -->
      <div class="flex flex-col md:flex-row gap-4 mb-4">
        <div class="flex-1">
          <label class="block text-sm font-medium mb-1">Buscar por nome</label>
          <InputText v-model="termoBusca" class="w-full" placeholder="Digite o nome..." />
        </div>
        <div class="flex-1">
          <label class="block text-sm font-medium mb-1">Buscar por CPF</label>
          <InputText v-model="filtroCpf" class="w-full" placeholder="Digite o CPF..." />
        </div>
      </div>
  
      <!-- Tabela de resultados -->
      <DataTable :value="usuariosFiltrados" :rows="5" paginator responsiveLayout="scroll">
        <Column field="nome" header="Nome" />
        <Column field="cpf" header="CPF" />
        <Column header="Grupo">
          <template #body="{ data }">
            <Dropdown
              v-model="selecaoGrupo[data.id]"
              :options="grupos"
              optionLabel="nome"
              optionValue="id"
              placeholder="Selecione"
              class="w-full"
            />
          </template>
        </Column>
        <Column header="Ação">
          <template #body="{ data }">
            <Button
              label="Vincular"
              icon="pi pi-check"
              class="p-button-sm p-button-success"
              @click="vincular(data.id)"
            />
          </template>
        </Column>
      </DataTable>
    </div>
  </template>