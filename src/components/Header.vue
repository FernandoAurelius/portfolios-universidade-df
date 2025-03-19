<template>
  <header class="bg-indigo-800 text-white shadow-lg">
    <div class="container mx-auto px-4 py-5 flex flex-col md:flex-row justify-between items-center">
      <div class="logo-container flex items-center mb-4 md:mb-0">
        <div class="spin-slow mr-3">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
          </svg>
        </div>
        <div>
          <h1 class="text-2xl font-bold">TechEdu DF</h1>
          <p class="text-sm text-indigo-200">Seu guia de educação em TI no Distrito Federal</p>
        </div>
      </div>
      
      <nav class="flex space-x-4 text-lg">
        <a href="#" class="hover:text-indigo-300 transition-colors duration-200 border-b-2 border-transparent hover:border-indigo-300 pb-1">Início</a>
        <a href="#universidades" class="hover:text-indigo-300 transition-colors duration-200 border-b-2 border-transparent hover:border-indigo-300 pb-1">Universidades</a>
        <a href="#sobre" class="hover:text-indigo-300 transition-colors duration-200 border-b-2 border-transparent hover:border-indigo-300 pb-1">Sobre</a>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header'
}
</script>
