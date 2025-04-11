<template>
  <div class="relative h-screen w-screen overflow-hidden">
    <!-- Main container with vertical slides -->
    <div
      ref="containerRef"
      class="absolute inset-0 flex flex-col"
      @wheel.prevent="handleWheel"
    >
      <div 
        v-for="(slide, index) in slides" 
        :key="index"
        class="flex-shrink-0 h-full w-full flex items-center justify-center relative"
      >
        <!-- Background image with motion -->
        <motion.div 
          :initial="{ opacity: index === 0 ? 1 : 0.6 }"
          :animate="{ 
            opacity: index === currentSlide ? 1 : 0.6,
            scale: index === currentSlide ? 1 : 0.9
          }"
          :transition="{ duration: 0.5 }"
          class="h-full w-full absolute inset-0 bg-black"
        >
          <img 
            src="/img/sample.png" 
            :alt="`Model ${index + 1}`" 
            class="object-cover h-full w-full"
          />
          <!-- Dark overlay -->
          <div class="absolute inset-0 bg-black/30"></div>
        </motion.div>
        
        <!-- Text content with motion -->
        <motion.div
          :initial="{ opacity: 0, y: 50 }"
          :animate="{ 
            opacity: index === currentSlide ? 1 : 0,
            y: index === currentSlide ? 0 : 50
          }"
          :transition="{ duration: 0.7, delay: 0.2 }"
          class="absolute z-10 text-white max-w-2xl px-8 text-center"
        >
          <h2 class="text-5xl font-light tracking-wider mb-4">{{ slide.title }}</h2>
          <p class="text-xl text-white/80 mb-8">{{ slide.description }}</p>
          <UButton 
            v-if="index === currentSlide" 
            color="white" 
            variant="outline" 
            class="mt-4"
          >
            View More
          </UButton>
        </motion.div>
      </div>
    </div>
    
    <!-- Agency logo -->
    <div class="absolute top-8 left-8 z-20">
      <motion.div
        :initial="{ opacity: 0 }"
        :animate="{ opacity: 1 }"
        :transition="{ duration: 1 }"
      >
        <h1 class="text-white text-3xl font-light tracking-[0.3em]">ELITE<span class="font-bold">MODELS</span></h1>
      </motion.div>
    </div>
    
    <!-- Navigation indicators -->
    <div class="absolute right-8 top-1/2 transform -translate-y-1/2 flex flex-col gap-3 z-20">
      <button 
        v-for="(_, index) in slides" 
        :key="index"
        @click="goToSlide(index)"
        class="w-3 h-3 rounded-full transition-all duration-300 ease-in-out"
        :class="index === currentSlide ? 'bg-white' : 'bg-white/50 hover:bg-white/70'"
      ></button>
    </div>
    
    <!-- Scroll indicator -->
    <motion.div
      :animate="{ y: [0, 10, 0] }"
      :transition="{ 
        repeat: Infinity, 
        duration: 2,
        ease: 'easeInOut'
      }"
      class="absolute bottom-10 left-1/2 transform -translate-x-1/2 z-20 text-white/70"
    >
      <div class="flex flex-col items-center">
        <span class="text-xs uppercase tracking-widest mb-2">Scroll</span>
        <Icon name="heroicons:arrow-down" class="w-5 h-5" />
      </div>
    </motion.div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { motion } from 'motion-v'

// Slides data - can be replaced with real data later
const slidesData = [
  { 
    id: 1, 
    alt: 'Model 1',
    title: 'Discover New Talent',
    description: 'EliteModels represents extraordinary talents from around the world, bringing fresh faces to international fashion runways and campaigns.'
  },
  { 
    id: 2, 
    alt: 'Model 2',
    title: 'Fashion Forward',
    description: 'Our models work with the most prestigious fashion houses and photographers, setting trends and defining the future of fashion.'
  },
  { 
    id: 3, 
    alt: 'Model 3',
    title: 'Join Our Agency',
    description: "We're always looking for new faces. Apply today to be represented by one of the most respected agencies in the industry."
  },
]

const slides = ref(slidesData)
const currentSlide = ref(0)
const containerRef = ref(null)
const isAnimating = ref(false)

const handleWheel = (event) => {
  if (isAnimating.value) return
  
  isAnimating.value = true
  setTimeout(() => { isAnimating.value = false }, 800) // Debounce scrolling
  
  const direction = event.deltaY > 0 ? 1 : -1
  const nextSlide = (currentSlide.value + direction + slides.value.length) % slides.value.length
  
  goToSlide(nextSlide)
}

const goToSlide = (index) => {
  currentSlide.value = index
  
  // Animate the container position
  if (containerRef.value) {
    containerRef.value.style.transform = `translateY(-${index * 100}%)`
    containerRef.value.style.transition = 'transform 0.8s ease-out'
  }
}

onMounted(() => {
  // Initialize container position
  if (containerRef.value) {
    containerRef.value.style.transition = 'transform 0.8s ease-out'
  }
})
</script>

<style scoped>
/* Smooth transitions for all animations */
.flex-col > div {
  transition: transform 0.8s ease-out;
}
</style> 