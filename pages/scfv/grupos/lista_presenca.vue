<script setup lang="ts">
import { ref } from 'vue'

const grupos = ref([
  { id: 1, nome: 'Grupo 1' },
  { id: 2, nome: 'Grupo 2' },
])

const grupoSelecionado = ref(null)
const dataPresenca = ref()
const membrosDoGrupo = ref([])
const presencas = ref<Record<number, boolean>>({})

function onGrupoChange() {
  // Simular carregamento da API
  membrosDoGrupo.value = [
    { id: 1, nome: 'João da Silva Fernandes', cpf: '00000000000' },
    { id: 2, nome: 'Maria Soares Lima', cpf: '11111111111' },
    { id: 3, nome: 'Carlos José Santiago', cpf: '22222222222' },
  ]
  presencas.value = {}
}

function marcarTodos(valor: boolean) {
  membrosDoGrupo.value.forEach(m => {
    presencas.value[m.id] = valor
  })
}

function enviarPresenca() {
  const presentes = Object.entries(presencas.value)
    .filter(([, presente]) => presente)
    .map(([id]) => parseInt(id))

  if (!grupoSelecionado.value || !dataPresenca.value) {
    alert('Selecione grupo e data antes de enviar.')
    return
  }

  console.log({
    grupo_id: grupoSelecionado.value,
    data: dataPresenca.value,
    presentes,
  })

  alert('Presença enviada!')
}
</script>

<template>
    <div class="p-4 space-y-4">
      <h2 class="text-2xl font-semibold mb-4 text-center uppercase">Lista de Presença</h2>
  
      <!-- Selecionar grupo -->
      <div>
        <label class="text-sm font-medium">Grupo</label>
        <Select
          v-model="grupoSelecionado"
          :options="grupos"
          optionLabel="nome"
          optionValue="id"
          placeholder="Selecione o grupo"
          class="w-full"
          @change="onGrupoChange"
        />
      </div>
  
      <!-- Selecionar data -->
      <div v-if="grupoSelecionado">
        <label class="text-sm font-medium">Data</label>
        <DatePicker v-model="dataPresenca" showIcon fluid iconDisplay="input" class="w-full" />
      </div>

        <div class="space-x-2 space-y-1 text-center">
          <Button label="Todos Presentes" size="small" @click="marcarTodos(true)" />
          <Button label="Todos Ausentes" size="small" severity="secondary" @click="marcarTodos(false)" />
        </div>

        <DataTable :value="membrosDoGrupo" showGridlines :rows="20" paginator responsiveLayout="scroll" class="w-full" >
          <Column field="nome" header="Nome" style="min-width: 8rem" />
          <Column field="cpf" header="CPF" style="min-width: 8rem" />
          <Column header="Presença" style="min-width: 6rem" >
            <template #body="{ data }">
              <Checkbox
                v-model="presencas[data.id]"
                :binary="true"
                class="scale-125"
              />
            </template>
          </Column>
        </DataTable>



      <!-- Lista de usuários -->
      <!--<div v-if="membrosDoGrupo.length > 0">
        <div class="flex justify-between items-center mb-2 space-x-4">
          <h3 class="text-md font-semibold">Participantes</h3>
          
        </div>
  
        <ul class="space-y-2">
          <li
            v-for="membro in membrosDoGrupo"
            :key="membro.id"
            class="flex justify-between items-center p-2 border rounded"
          >
            <span class="truncate">{{ membro.nome }}</span>
            <Checkbox
              v-model="presencas[membro.id]"
              :binary="true"
              class="scale-125"
            />
          </li>
        </ul>-->
        <div>
  
        <div class="flex gap-2 mt-4">
          <Button
            label="Enviar presença"
            icon="pi pi-check"
            class="p-button-success"
            @click="enviarPresenca"
          />
          <Button
            label="Gerar PDF"
            icon="pi pi-file-pdf"
            class="p-button-secondary"
            disabled
            title="Em breve"
          />
        </div>
      </div>
    </div>
  </template>