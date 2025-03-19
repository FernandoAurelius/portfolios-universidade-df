<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4" @click.self="close">
    <div 
      class="bg-white rounded-2xl shadow-2xl w-full max-w-3xl max-h-[90vh] overflow-y-auto transform transition-all duration-300 scale-in"
      @click.stop
    >
      <div :class="[universidade.cor, 'h-2 rounded-t-2xl']"></div>
      
      <div class="p-6">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-2xl md:text-3xl font-bold text-gray-800">{{ universidade.sigla }}</h2>
            <p class="text-gray-600">{{ universidade.nome }}</p>
          </div>
          <button 
            @click="close" 
            class="p-2 rounded-full hover:bg-gray-200 transition-colors duration-200"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
        
        <div class="mt-6 grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="space-y-4">
            <div class="bg-indigo-50 p-4 rounded-lg">
              <h3 class="text-lg font-medium text-indigo-800 mb-2">Cursos Oferecidos</h3>
              <ul class="space-y-2">
                <li 
                  v-for="(curso, index) in universidade.cursos" 
                  :key="index"
                  class="flex items-center"
                >
                  <span class="inline-block w-2 h-2 rounded-full mr-2" :class="universidade.cor"></span>
                  {{ curso }}
                </li>
              </ul>
            </div>
            
            <div>
              <h3 class="text-lg font-medium text-gray-700 mb-2">Informações</h3>
              <div class="grid grid-cols-2 gap-4">
                <div class="bg-gray-50 p-3 rounded-lg">
                  <span class="block text-sm text-gray-500">Grau</span>
                  <span class="font-medium text-gray-800">{{ universidade.grau }}</span>
                </div>
                <div class="bg-gray-50 p-3 rounded-lg">
                  <span class="block text-sm text-gray-500">Modalidade</span>
                  <span class="font-medium text-gray-800">{{ universidade.modalidade }}</span>
                </div>
              </div>
            </div>
          </div>
          
          <div class="bg-gray-50 p-4 rounded-lg">
            <h3 class="text-lg font-medium text-gray-700 mb-2">Observações</h3>
            <p class="text-gray-700">
              {{ universidade.observacoes }}
            </p>
            
            <div class="mt-6">
              <h4 class="text-md font-medium text-gray-700 mb-2">Mais sobre o curso</h4>
              <p class="text-gray-600 text-sm">
                A área de Tecnologia da Informação (TI) no Distrito Federal é bastante diversificada – abrangendo desde cursos mais teóricos até formações mais aplicadas. As instituições oferecem cursos com diferentes graus e modalidades, refletindo a crescente demanda do mercado por profissionais capacitados em inovação digital, gestão tecnológica e desenvolvimento de sistemas.
              </p>
            </div>
            
            <div class="mt-6">
              <button class="px-4 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 transition-colors duration-200">
                Visitar site da instituição
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
    
    return {
      close
    }
  }
}
</script>
