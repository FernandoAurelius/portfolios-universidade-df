<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4 overflow-y-auto" @click.self="close">
    <div 
      class="bg-white rounded-2xl shadow-2xl w-full max-w-4xl max-h-[90vh] overflow-y-auto transform transition-all duration-300 scale-in my-8"
      @click.stop
    >
      <div :class="[universidade.cor, 'h-2 rounded-t-2xl']"></div>
      
      <div class="p-4 sm:p-6">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-xl md:text-3xl font-bold text-gray-800">{{ universidade.sigla }}</h2>
            <p class="text-sm sm:text-base text-gray-600">{{ universidade.nome }}</p>
          </div>
          <button 
            @click="close" 
            class="p-2 rounded-full hover:bg-gray-200 transition-colors duration-200"
            aria-label="Fechar"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
        
        <div class="mt-6 grid grid-cols-1 lg:grid-cols-3 gap-6">
          <div class="col-span-2 space-y-6">
            <div class="bg-indigo-50 p-4 rounded-lg">
              <h3 class="text-lg font-medium text-indigo-800 mb-4">Cursos Disponíveis</h3>
              
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div 
                  v-for="curso in getCursosOrdenados()" 
                  :key="curso.nome"
                  class="bg-white p-3 rounded-lg shadow-sm border-l-4 transition-all hover:shadow-md"
                  :class="getBorderColorForCurso(curso.tipo_de_graduacao)"
                >
                  <div class="flex items-center">
                    <div class="w-8 h-8 rounded-full mr-2 flex items-center justify-center"
                      :class="getBackgroundForCurso(curso.tipo_de_graduacao)">
                      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path d="M12 14l9-5-9-5-9 5 9 5z" />
                        <path d="M12 14l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14z" />
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 14l9-5-9-5-9 5 9 5zm0 0l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14zm-4 6v-7.5l4-2.222" />
                      </svg>
                    </div>
                    <div class="flex-1">
                      <h4 class="font-medium text-gray-800">{{ curso.nome }}</h4>
                      <span class="text-xs rounded-full px-2 py-1 inline-block mt-1"
                        :class="getTagColorForCurso(curso.tipo_de_graduacao)">
                        {{ curso.tipo_de_graduacao }}
                      </span>
                    </div>
                  </div>
                  
                  <div class="mt-3 text-sm text-gray-600 flex flex-col gap-2">
                    <div>
                      <span class="font-medium">Modalidade:</span>
                      <span class="ml-1">{{ curso.modalidade }}</span>
                    </div>
                    <div>
                      <span class="font-medium">Duração:</span>
                      <span class="ml-1">{{ curso.duracao }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="bg-gray-50 p-4 rounded-lg">
              <h3 class="text-lg font-medium text-gray-700 mb-3">Observações</h3>
              <p class="text-gray-700">
                {{ universidade.observacoes }}
              </p>
            </div>
          </div>
          
          <div class="space-y-6">
            <div class="bg-gradient-to-br from-indigo-50 to-blue-50 p-4 rounded-lg">
              <h3 class="text-lg font-medium text-indigo-800 mb-3">Informações Adicionais</h3>
              
              <div class="space-y-4">
                <div>
                  <h4 class="text-md font-medium text-gray-700 mb-2">Tipos de Curso</h4>
                  <div class="flex flex-wrap gap-2">
                    <span v-for="tipo in getTiposCursos()" :key="tipo"
                      class="inline-block px-3 py-1 text-sm rounded-full"
                      :class="getTagColorForCurso(tipo)">
                      {{ tipo }}
                    </span>
                  </div>
                </div>
                
                <div>
                  <h4 class="text-md font-medium text-gray-700 mb-2">Modalidades</h4>
                  <div class="flex flex-wrap gap-2">
                    <span v-for="modalidade in getModalidadesUnicas()" :key="modalidade"
                      class="inline-block px-3 py-1 text-sm bg-blue-100 text-blue-800 rounded-full">
                      {{ modalidade }}
                    </span>
                  </div>
                </div>
                
                <div>
                  <h4 class="text-md font-medium text-gray-700 mb-2">Sobre a Instituição</h4>
                  <p class="text-gray-600 text-sm">
                    A área de Tecnologia da Informação no DF é bastante diversificada, com instituições oferecendo diferentes modalidades e graus. 
                    {{ universidade.nome }} se destaca por oferecer formações que atendem às necessidades do mercado.
                  </p>
                </div>
              </div>
            </div>
            
            <div class="flex flex-col">
              <a 
                :href="universidade.link" 
                target="_blank"
                class="px-4 py-3 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition-colors duration-300 text-center font-medium"
                rel="noopener noreferrer"
              >
                <span class="flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                  </svg>
                  Visitar site da instituição
                </span>
              </a>
              
              <button 
                @click="close"
                class="mt-3 px-4 py-2 border border-gray-300 bg-white text-gray-700 rounded-lg hover:bg-gray-100 transition-colors duration-300 text-center font-medium"
              >
                Fechar
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UniversidadeModal',
  props: {
    universidade: {
      type: Object,
      required: true
    }
  },
  setup(props, { emit }) {
    const close = () => {
      emit('close')
    }
    
    const getCursosOrdenados = () => {
      return [...props.universidade.cursos]
        .filter(curso => curso.tipo_de_graduacao !== 'Não ofertado' && curso.modalidade !== 'Não ofertado')
        .sort((a, b) => {
          const order = { 'Bacharelado': 1, 'Licenciatura': 2, 'Tecnólogo': 3 }
          return order[a.tipo_de_graduacao] - order[b.tipo_de_graduacao]
        })
    }
    
    const getTiposCursos = () => {
      const tipos = new Set()
      props.universidade.cursos.forEach(curso => {
        if (curso.tipo_de_graduacao !== 'Não ofertado') {
          tipos.add(curso.tipo_de_graduacao)
        }
      })
      return [...tipos]
    }
    
    const getModalidadesUnicas = () => {
      const modalidades = new Set()
      props.universidade.cursos.forEach(curso => {
        if (curso.modalidade !== 'Não ofertado' && curso.tipo_de_graduacao !== 'Não ofertado') {
          curso.modalidade.split('/').forEach(m => modalidades.add(m.trim()))
        }
      })
      return [...modalidades]
    }
    
    const getTagColorForCurso = (tipo) => {
      switch (tipo) {
        case 'Bacharelado':
          return 'bg-blue-100 text-blue-800'
        case 'Licenciatura':
          return 'bg-green-100 text-green-800'
        case 'Tecnólogo':
          return 'bg-purple-100 text-purple-800'
        default:
          return 'bg-gray-100 text-gray-800'
      }
    }
    
    const getBackgroundForCurso = (tipo) => {
      switch (tipo) {
        case 'Bacharelado':
          return 'bg-blue-50 text-blue-600'
        case 'Licenciatura':
          return 'bg-green-50 text-green-600'
        case 'Tecnólogo':
          return 'bg-purple-50 text-purple-600'
        default:
          return 'bg-gray-50 text-gray-600'
      }
    }
    
    const getBorderColorForCurso = (tipo) => {
      switch (tipo) {
        case 'Bacharelado':
          return 'border-blue-400'
        case 'Licenciatura':
          return 'border-green-400'
        case 'Tecnólogo':
          return 'border-purple-400'
        default:
          return 'border-gray-400'
      }
    }
    
    return {
      close,
      getCursosOrdenados,
      getTiposCursos,
      getModalidadesUnicas,
      getTagColorForCurso,
      getBackgroundForCurso,
      getBorderColorForCurso
    }
  }
}
</script>
