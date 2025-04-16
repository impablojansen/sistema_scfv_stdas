<script setup lang="ts">
const form = ref({
    nome: "",
    descricao: "",
    faixa_etaria: "",
    turno: "",
    dias_semana: [],
    cras_id: null
})

const faixasEtarias = [
  { label: '00 a 06 anos', value: '00-06' },
  { label: '07 a 14 anos', value: '07-14' },
  { label: '15 a 17 anos', value: '15-17' },
  { label: '18 a 59 anos', value: '18-59' },
  { label: '60 anos ou mais', value: '60+' },
]

const turnos = [
  { label: 'Manhã', value: 'manha'},
  { label: 'Tarde', value: 'tarde'},
]

const diasSemana = [
  { label: 'Segunda-feira', value: 'segunda' },
  { label: 'Terça-feira', value: 'terca' },
  { label: 'Quarta-feira', value: 'quarta' },
  { label: 'Quinta-feira', value: 'quinta' },
  { label: 'Sexta-feira', value: 'sexta' },
]

const crasList = [
  { id: 1, nome: 'CRAS Alto Luminoso' },
  { id: 2, nome: 'CRAS Caponga' },
  { id: 3, nome: 'CRAS Guanacés' },
  { id: 4, nome: 'CRAS Planalto' },
]

const submitForm = () => {
    console.log('Formulário enviado:', form.value)
}

</script>
<template>
    <!--
      <div class="max-w-2xl mx-auto mt-8 p-4 bg-white rounded-lg shadow-md">
    
    -->
        <div>
      <h1 class="text-2xl font-semibold mb-6 text-gray-700 text-center">Grupo do SCFV</h1>
  
      <form @submit.prevent="submitForm" class="space-y-4">
        <!-- CRAS -->
        <div>
          <label for="cras" class="block text-sm font-medium text-gray-700 mb-1">CRAS</label>
          <Select
            id="cras"
            v-model="form.cras_id"
            :options="crasList"
            optionLabel="nome"
            optionValue="id"
            placeholder="Selecione o CRAS"
            class="w-full"
          />
        </div>

        <!-- Nome -->
        <div>
          <label for="nome" class="block text-sm font-medium text-gray-700 mb-1">Nome do grupo</label>
          <InputText id="nome" v-model="form.nome" class="w-full" placeholder="Digite o nome do grupo" />
        </div>
  
        <!-- Faixa etária -->
        <div>
          <label for="faixaEtaria" class="block text-sm font-medium text-gray-700 mb-1">Faixa etária</label>
          <Select
            id="faixaEtaria"
            v-model="form.faixa_etaria"
            :options="faixasEtarias"
            optionLabel="label"
            optionValue="value"
            placeholder="Selecione a faixa etária"
            class="w-full"
          />
        </div>
  
        <!-- Turno -->
        <div>
          <label for="turno" class="block text-sm font-medium text-gray-700 mb-1">Turno</label>
          <Select
            id="turno"
            v-model="form.turno"
            :options="turnos"
            optionLabel="label"
            optionValue="value"
            placeholder="Selecione o turno"
            class="w-full"
          />
        </div>
        
        <!-- Dias da semana -->
        <div>
          <label for="diasSemana" class="block text-sm font-medium text-gray-700 mb-1">Dias da semana</label>
          <MultiSelect
            id="diasSemana"
            v-model="form.dias_semana"
            :options="diasSemana"
            optionLabel="label"
            optionValue="value"
            placeholder="Selecione os dias de encontro"
            class="w-full"
          />
        </div>
  
  
        <!-- Botão -->
         <div class="justify-items items-center text-center">
           <Button type="submit" label="Salvar grupo" icon="pi pi-save" class="w-full sm:w-auto" />
         </div>
      </form>
    </div>
  </template>