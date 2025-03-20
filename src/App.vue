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
        :duracoes="getDuracoes()"
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
      modalidade: '',
      duracao: ''
    })

    const filteredUniversidades = computed(() => {
      let result = [...universidades]
      
      // Filtrar por texto de busca
      if (filters.value.search) {
        const searchTerm = filters.value.search.toLowerCase()
        result = result.filter(uni => 
          uni.nome.toLowerCase().includes(searchTerm) || 
          uni.sigla.toLowerCase().includes(searchTerm) ||
          uni.cursos.some(curso => 
            curso.tipo_de_graduacao !== 'Não ofertado' && 
            curso.nome.toLowerCase().includes(searchTerm)
          ) ||
          uni.observacoes.toLowerCase().includes(searchTerm)
        )
      }
      
      // Filtrar por curso
      if (filters.value.curso) {
        result = result.filter(uni => 
          uni.cursos.some(curso => 
            curso.tipo_de_graduacao !== 'Não ofertado' && 
            curso.nome.toLowerCase().includes(filters.value.curso.toLowerCase())
          )
        )
      }
      
      // Filtrar por grau
      if (filters.value.grau) {
        result = result.filter(uni => 
          uni.cursos.some(curso => 
            curso.tipo_de_graduacao !== 'Não ofertado' && 
            curso.tipo_de_graduacao.includes(filters.value.grau)
          )
        )
      }
      
      // Filtrar por modalidade
      if (filters.value.modalidade) {
        result = result.filter(uni => 
          uni.cursos.some(curso => 
            curso.tipo_de_graduacao !== 'Não ofertado' && 
            curso.modalidade.includes(filters.value.modalidade)
          )
        )
      }
      
      // Filtrar por duração
      if (filters.value.duracao) {
        result = result.filter(uni => 
          uni.cursos.some(curso => 
            curso.tipo_de_graduacao !== 'Não ofertado' && 
            curso.duracao.includes(filters.value.duracao)
          )
        )
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
        uni.cursos.forEach(curso => {
          if (curso.tipo_de_graduacao !== 'Não ofertado') {
            cursos.add(curso.nome)
          }
        })
      })
      return [...cursos].sort()
    }

    const getGraus = () => {
      const graus = new Set()
      universidades.forEach(uni => {
        uni.cursos.forEach(curso => {
          if (curso.tipo_de_graduacao !== 'Não ofertado') {
            graus.add(curso.tipo_de_graduacao)
          }
        })
      })
      return [...graus].sort()
    }

    const getModalidades = () => {
      const modalidades = new Set()
      universidades.forEach(uni => {
        uni.cursos.forEach(curso => {
          if (curso.tipo_de_graduacao !== 'Não ofertado' && curso.modalidade !== 'Não ofertado') {
            curso.modalidade.split('/').forEach(m => modalidades.add(m.trim()))
          }
        })
      })
      return [...modalidades].sort()
    }

    const getDuracoes = () => {
      const duracoes = new Set()
      universidades.forEach(uni => {
        uni.cursos.forEach(curso => {
          if (curso.tipo_de_graduacao !== 'Não ofertado' && curso.duracao !== 'N/A') {
            duracoes.add(curso.duracao)
          }
        })
      })
      return [...duracoes].sort((a, b) => {
        // Ordenar por número de anos
        const numA = parseFloat(a.replace(',', '.'));
        const numB = parseFloat(b.replace(',', '.'));
        return numA - numB;
      })
    }

    return {
      selectedUniversidade,
      filteredUniversidades,
      applyFilters,
      openUniversidadeDetails,
      getCursos,
      getGraus,
      getModalidades,
      getDuracoes
    }
  }
}
</script>
