<template>
  <div id="universidades">
    <div class="flex justify-between items-center mb-6">
      <h2 class="text-2xl font-bold text-indigo-800">{{ universidades.length }} Instituições encontradas</h2>
      <div class="flex space-x-4">
        <button 
          @click="viewMode = 'grid'" 
          class="p-2 rounded-md transition-all" 
          :class="viewMode === 'grid' ? 'bg-indigo-100 text-indigo-800' : 'bg-gray-100 text-gray-600'"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
          </svg>
        </button>
        <button 
          @click="viewMode = 'list'" 
          class="p-2 rounded-md transition-all" 
          :class="viewMode === 'list' ? 'bg-indigo-100 text-indigo-800' : 'bg-gray-100 text-gray-600'"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
      </div>
    </div>
    
    <div v-if="universidades.length === 0" class="text-center py-16">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M12 13.5V10m0 0L9.5 7.5m2.5 2.5l2.5-2.5" />
      </svg>
      <h3 class="text-xl font-medium text-gray-700 mt-4">Nenhuma instituição encontrada</h3>
      <p class="text-gray-500">Tente ajustar seus filtros para encontrar resultados.</p>
    </div>
    
    <transition-group
      name="staggered-fade"
      tag="div"
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
    >
      <template v-if="viewMode === 'grid'">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div 
            v-for="(uni, index) in universidades" 
            :key="uni.id"
            :data-index="index"
            class="bg-white rounded-xl shadow-lg overflow-hidden hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 cursor-pointer"
            @click="viewDetails(uni)"
          >
            <div :class="[uni.cor, 'h-3']"></div>
            <div class="p-6">
              <div class="flex justify-between items-start">
                <div>
                  <h3 class="text-xl font-bold text-gray-800">{{ uni.sigla }}</h3>
                  <p class="text-sm text-gray-600">{{ uni.nome }}</p>
                </div>
                <div class="rounded-full bg-indigo-100 p-2 pulse">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                </div>
              </div>
              
              <div class="mt-4">
                <h4 class="text-sm font-medium text-gray-700">Cursos:</h4>
                <div class="flex flex-wrap gap-2 mt-1">
                  <span 
                    v-for="(curso, i) in uni.cursos.slice(0, 2)" 
                    :key="i" 
                    class="inline-block px-2 py-1 text-xs bg-indigo-100 text-indigo-800 rounded-md"
                  >
                    {{ curso }}
                  </span>
                  <span 
                    v-if="uni.cursos.length > 2" 
                    class="inline-block px-2 py-1 text-xs bg-gray-100 text-gray-800 rounded-md"
                  >
                    +{{ uni.cursos.length - 2 }}
                  </span>
                </div>
              </div>
              
              <div class="flex justify-between mt-4 text-sm text-gray-700">
                <div>
                  <span class="font-medium">Grau:</span> {{ uni.grau }}
                </div>
                <div>
                  <span class="font-medium">Modalidade:</span> {{ uni.modalidade }}
                </div>
              </div>
              
              <button 
                class="w-full mt-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition-colors duration-300"
                @click.stop="viewDetails(uni)"
              >
                Ver Detalhes
              </button>
            </div>
          </div>
        </div>
      </template>
      
      <template v-else>
        <div class="space-y-4">
          <div 
            v-for="(uni, index) in universidades" 
            :key="uni.id"
            :data-index="index"
            class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-all duration-300 flex"
            @click="viewDetails(uni)"
          >
            <div :class="[uni.cor, 'w-3']"></div>
            <div class="flex-grow p-4">
              <div class="flex justify-between items-center">
                <div>
                  <h3 class="text-lg font-bold text-gray-800">{{ uni.sigla }} - {{ uni.nome }}</h3>
                </div>
                <div class="hidden md:block">
                  <div class="flex space-x-2">
                    <span 
                      v-for="(curso, i) in uni.cursos.slice(0, 2)" 
                      :key="i" 
                      class="inline-block px-2 py-1 text-xs bg-indigo-100 text-indigo-800 rounded-md"
                    >
                      {{ curso }}
                    </span>
                    <span 
                      v-if="uni.cursos.length > 2" 
                      class="inline-block px-2 py-1 text-xs bg-gray-100 text-gray-800 rounded-md"
                    >
                      +{{ uni.cursos.length - 2 }}
                    </span>
                  </div>
                </div>
              </div>
              
              <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-2">
                <div class="col-span-2">
                  <p class="text-sm text-gray-600">
                    <span class="font-medium">Observações:</span> {{ uni.observacoes }}
                  </p>
                </div>
                <div class="flex md:justify-end items-center space-x-4">
                  <div class="text-sm">
                    <span class="text-gray-700 font-medium">{{ uni.grau }}</span>
                    <span class="mx-1">•</span>
                    <span class="text-gray-700">{{ uni.modalidade }}</span>
                  </div>
                  <button 
                    class="px-4 py-1 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 transition-colors duration-300"
                    @click.stop="viewDetails(uni)"
                  >
                    Detalhes
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </transition-group>
  </div>
</template>

<script>
import { ref } from 'vue'
import gsap from 'gsap'

export default {
  name: 'UniversidadesList',
  props: {
    universidades: {
      type: Array,
      required: true
    }
  },
  setup(props, { emit }) {
    const viewMode = ref('grid')

    const viewDetails = (universidade) => {
      emit('view-details', universidade)
    }

    // Animações com GSAP
    const beforeEnter = (el) => {
      el.style.opacity = 0
      el.style.transform = 'translateY(30px)'
    }

    const enter = (el, done) => {
      const delay = el.dataset.index * 0.1
      gsap.to(el, {
        opacity: 1,
        y: 0,
        duration: 0.6,
        delay: delay,
        ease: 'power2.out',
        onComplete: done
      })
    }

    return {
      viewMode,
      viewDetails,
      beforeEnter,
      enter
    }
  }
}
</script>
