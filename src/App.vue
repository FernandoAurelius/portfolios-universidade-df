<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
    <Header />
    
    <main class="container mx-auto px-4 py-8">
      <div class="fade-in">
        <h1 class="text-4xl md:text-5xl font-bold mb-4 text-indigo-800 text-center">
          Universidades de TI no Distrito Federal
        </h1>
        <p class="text-lg text-gray-700 max-w-4xl mx-auto text-center mb-8">
          Explore as principais instituições de ensino superior que oferecem cursos na área de Tecnologia da Informação no DF.
        </p>
      </div>
      
      <SearchFilter 
        @filter="applyFilters" 
        :cursos="getCursos()" 
        :graus="getGraus()" 
        :modalidades="getModalidades()" 
        class="slide-up"
      />
      
      <UniversidadesList 
        :universidades="filteredUniversidades" 
        @view-details="openUniversidadeDetails" 
        class="rise-up"
      />
    </main>
    
    <UniversidadeModal 
      v-if="selectedUniversidade" 
      :universidade="selectedUniversidade" 
      @close="selectedUniversidade = null" 
    />
    
    <Footer />
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import SearchFilter from './components/SearchFilter.vue'
import UniversidadesList from './components/UniversidadesList.vue'
import UniversidadeModal from './components/UniversidadeModal.vue'
import { universidades } from './data/universidades'

export default {
  components: {
    Header,
    Footer,
    SearchFilter,
    UniversidadesList,
    UniversidadeModal
  },
  setup() {
    const selectedUniversidade = ref(null)
    const filters = ref({
      search: '',
      curso: '',
      grau: '',
      modalidade: ''
    })

    const filteredUniversidades = computed(() => {
      let result = [...universidades]
      
      // Filtrar por texto de busca
      if (filters.value.search) {
        const searchTerm = filters.value.search.toLowerCase()
        result = result.filter(uni => 
          uni.nome.toLowerCase().includes(searchTerm) || 
          uni.sigla.toLowerCase().includes(searchTerm) ||
          uni.cursos.some(curso => curso.toLowerCase().includes(searchTerm)) ||
          uni.observacoes.toLowerCase().includes(searchTerm)
        )
      }
      
      // Filtrar por curso
      if (filters.value.curso) {
        result = result.filter(uni => 
          uni.cursos.some(curso => curso.toLowerCase().includes(filters.value.curso.toLowerCase()))
        )
      }
      
      // Filtrar por grau
      if (filters.value.grau) {
        result = result.filter(uni => uni.grau.includes(filters.value.grau))
      }
      
      // Filtrar por modalidade
      if (filters.value.modalidade) {
        result = result.filter(uni => uni.modalidade.includes(filters.value.modalidade))
      }
      
      return result
    })

    const applyFilters = (newFilters) => {
      filters.value = { ...newFilters }
    }

    const openUniversidadeDetails = (universidade) => {
      selectedUniversidade.value = universidade
    }

    const getCursos = () => {
      const cursos = new Set()
      universidades.forEach(uni => {
        uni.cursos.forEach(curso => cursos.add(curso))
      })
      return [...cursos]
    }

    const getGraus = () => {
      const graus = new Set()
      universidades.forEach(uni => {
        uni.grau.split('/').forEach(g => graus.add(g.trim()))
      })
      return [...graus]
    }

    const getModalidades = () => {
      const modalidades = new Set()
      universidades.forEach(uni => {
        uni.modalidade.split('e').forEach(m => modalidades.add(m.trim()))
      })
      return [...modalidades]
    }

    return {
      selectedUniversidade,
      filteredUniversidades,
      applyFilters,
      openUniversidadeDetails,
      getCursos,
      getGraus,
      getModalidades
    }
  }
}
</script>
