<template>
  <div class="bg-white rounded-lg shadow-lg p-6 mb-8">
    <h2 class="text-2xl font-bold text-gray-800 mb-4">Encontre o curso ideal</h2>
    
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
      <div class="search-input-container">
        <label for="search" class="block text-gray-700 mb-2">Pesquisa por texto</label>
        <div class="relative">
          <input 
            id="search" 
            type="text" 
            v-model="filters.search" 
            placeholder="Pesquise por universidade ou curso..." 
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all duration-300"
            @input="emitFilters"
          >
          <span class="absolute right-3 top-2 text-gray-400">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </span>
        </div>
      </div>
      
      <div>
        <label for="curso" class="block text-gray-700 mb-2">Curso</label>
        <select 
          id="curso" 
          v-model="filters.curso" 
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all duration-300"
          @change="emitFilters"
        >
          <option value="">Todos os cursos</option>
          <option v-for="(curso, index) in cursos" :key="index" :value="curso">{{ curso }}</option>
        </select>
      </div>
      
      <div>
        <label for="grau" class="block text-gray-700 mb-2">Grau</label>
        <select 
          id="grau" 
          v-model="filters.grau" 
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all duration-300"
          @change="emitFilters"
        >
          <option value="">Todos os graus</option>
          <option v-for="(grau, index) in graus" :key="index" :value="grau">{{ grau }}</option>
        </select>
      </div>
      
      <div>
        <label for="modalidade" class="block text-gray-700 mb-2">Modalidade</label>
        <select 
          id="modalidade" 
          v-model="filters.modalidade" 
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all duration-300"
          @change="emitFilters"
        >
          <option value="">Todas as modalidades</option>
          <option v-for="(modalidade, index) in modalidades" :key="index" :value="modalidade">{{ modalidade }}</option>
        </select>
      </div>
    </div>
    
    <div class="flex justify-center mt-6">
      <button 
        @click="resetFilters" 
        class="px-6 py-2 bg-gray-200 hover:bg-gray-300 text-gray-800 rounded-lg transition-all duration-300 transform hover:scale-105"
      >
        Limpar filtros
      </button>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue'

export default {
  name: 'SearchFilter',
  props: {
    cursos: Array,
    graus: Array,
    modalidades: Array
  },
  setup(props, { emit }) {
    const filters = reactive({
      search: '',
      curso: '',
      grau: '',
      modalidade: ''
    })
    
    const emitFilters = () => {
      emit('filter', { ...filters })
    }
    
    const resetFilters = () => {
      filters.search = ''
      filters.curso = ''
      filters.grau = ''
      filters.modalidade = ''
      emitFilters()
    }
    
    return {
      filters,
      emitFilters,
      resetFilters
    }
  }
}
</script>
