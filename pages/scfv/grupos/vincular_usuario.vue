<script setup lang="ts">
const termoBusca = ref('')
const filtroCpf = ref('')

const grupoSelecionado = ref(null)
const membrosDoGrupo = ref([])

function onGrupoChange() {
  membrosDoGrupo.value = [
    { id: 1, nome: "João", cpf: '22133144155' },
  ]
}

const usuarios = ref([
  { id: 1, nome: 'Ana Beatriz Lima Costa', cpf: '11111111111' },
  { id: 2, nome: 'Ana Paula', cpf: '22222222222' },
  { id: 3, nome: 'João Carlos', cpf: '00000000000' },
  { id: 4, nome: 'João Pedro', cpf: '33333333333' },
  { id: 5, nome: 'Maria Silva', cpf: '44444444444' },
  { id: 6, nome: 'Maria José', cpf: '12345678912' },
  { id: 7, nome: 'José Costa', cpf: '98765432112' },
  { id: 8, nome: 'Carlos Miguel', cpf: '12312312312' },
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
    <div class="p-4 space-y-4">
      <h1 class="text-2xl font-semibold mb-4 text-center uppercase">Vincular usuário a grupo</h1>
  
      <div>
        <label class="text-sm font-medium">Grupo</label>
        <Dropdown 
          v-model="grupoSelecionado"
          :options="grupos"
          optionLabel="nome"
          optionValue="id"
          placeholder="Selecione o grupo"
          class="w-full"
          @change="onGrupoChange"
        />
      </div>
      
      <!-- Filtros de busca -->
      
      <div v-if="grupoSelecionado" class="space-y-4">
        <div>
          <label class="text-sm font-medium">Buscar por nome</label>
          <InputText v-model="termoBusca" class="w-full" placeholder="Digite o nome" />
        </div>
        
        <div>
          <label class="text-sm font-medium">Buscar por nome</label>
          <InputText v-model="filtroCpf" class="w-full" placeholder="Digite o CPF" />
        </div>
        
        <h2 class="text-xl font-semibold text-center uppercase">VINCULAR</h2>
        <!-- Tabela de resultados -->
        <DataTable :value="usuariosFiltrados" :rows="5" paginator >
          <Column field="nome" header="Nome" />
          <Column field="cpf" header="CPF" />
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
      
      <div v-if="membrosDoGrupo.length > 0" class="space-y-4">
        <h2 class="text-xl font-semibold text-center uppercase">USUÁRIOS INSCRITOS NO GRUPO {{ grupoSelecionado }}</h2>
  
        <!-- Mostrar usuários inscritos no grupo -->
        <DataTable :value="membrosDoGrupo" :rows="20" paginator >
          <Column field="nome" header="Nome" />
          <Column field="cpf" header="CPF" />
          <Column header="Ação">
            <template #body="{ data }">
              <div class="space-x-2 space-y-2 items-center justify-center text-center">
                <Button 
                  label="Desvincular"
                  icon="pi pi-times"
                  class="p-button-sm p-button-danger"
                />
              </div>
            </template>
          </Column>
        </DataTable>
      </div>
    </div>
  </template>