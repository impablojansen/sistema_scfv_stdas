<script setup lang="ts">
import { ref } from 'vue'

// Simulação de dados (futuramente vem da API)
const grupos = ref([
  {
    id: 1,
    nome: 'Grupo 1',
    descricao: 'Grupo de adolescentes',
    faixa_etaria: '12 a 17 anos',
    dias_semana: ['Terça', 'Quinta'],
    ativo: true
  },
  {
    id: 2,
    nome: 'Grupo 2',
    descricao: 'Grupo de idosos',
    faixa_etaria: '60+',
    dias_semana: ['Segunda', 'Quarta'],
    ativo: true
  },
])

const nomeBusca = ref('')
const faixaEtariaFiltro = ref(null)

const faixasEtarias = [
  { label: '0 a 6 anos', value: '0 a 6 anos' },
  { label: '6 a 12 anos', value: '6 a 12 anos' },
  { label: '12 a 17 anos', value: '12 a 17 anos' },
  { label: '18 a 59 anos', value: '18 a 59 anos' },
  { label: '60+', value: '60+' },
]

// Filtro simples
const gruposFiltrados = computed(() => {
  return grupos.value.filter(g =>
    (!nomeBusca.value || g.nome.toLowerCase().includes(nomeBusca.value.toLowerCase())) &&
    (!faixaEtariaFiltro.value || g.faixa_etaria === faixaEtariaFiltro.value)
  )
})
</script>


<template>
    <div class="p-4">
      <h2 class="text-2xl font-semibold mb-4">Grupos do SCFV</h2>
  
      <!-- Filtros -->
      <div class="flex flex-col md:flex-row gap-4 mb-4">
        <div class="flex-1">
          <label class="block text-sm font-medium mb-1">Buscar por nome</label>
          <InputText v-model="nomeBusca" class="w-full" placeholder="Digite o nome do grupo..." />
        </div>
        <div class="flex-1">
          <label class="block text-sm font-medium mb-1">Filtrar por faixa etária</label>
          <Dropdown v-model="faixaEtariaFiltro" :options="faixasEtarias" optionLabel="label" optionValue="value" placeholder="Selecione" class="w-full" />
        </div>
      </div>
  
      <!-- Tabela -->
      <DataTable :value="gruposFiltrados" paginator :rows="5" responsiveLayout="scroll">
        <Column field="nome" header="Nome do grupo" />
        <Column field="descricao" header="Descrição" />
        <Column field="faixa_etaria" header="Faixa Etária" />
        <Column field="dias_semana" header="Dias da semana" body="{row => row.dias_semana.join(', ')}" />
        <Column header="Ações" :body="row => h('Button', { label: 'Editar', icon: 'pi pi-pencil', class: 'p-button-sm p-button-secondary' })" />
      </DataTable>
    </div>
  </template>