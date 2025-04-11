<template>
  <div class="relative h-screen overflow-hidden bg-black">
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
    
    <!-- Ticker Container -->
    <div 
      class="relative h-screen flex items-center"
      @wheel.prevent="handleWheel"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      <!-- Main scrollable row with all images side by side -->
      <motion.div
        ref="containerRef"
        class="flex flex-row h-screen items-center"
        :initial="{ x: 0 }"
        :animate="{ x: containerX }"
        :transition="{ type: 'spring', stiffness: 300, damping: 35 }"
      >
        <div 
          v-for="(slide, index) in slides" 
          :key="index"
          class="flex-shrink-0 h-screen w-auto relative"
          @click="setCurrentSlide(index)"
        >
          <!-- Model image -->
          <div 
            class="h-full w-auto overflow-hidden relative"
            :class="index === currentSlide ? 'ring-2 ring-white/50' : ''"
          >
            <img 
              src="/img/sample.png" 
              :alt="`Model ${index + 1}`" 
              class="h-full w-auto object-cover"
            />
            
            <!-- Hover/selected overlay -->
            <div 
              class="absolute inset-0 bg-black/30 transition-opacity duration-300"
              :class="index === currentSlide ? 'opacity-0' : 'opacity-70 hover:opacity-30'"
            ></div>
            
            <!-- Model name -->
            <div class="absolute bottom-8 left-4 text-white">
              <h3 class="text-lg font-semibold">{{ slide.modelName }}</h3>
              <p class="text-sm text-white/80">{{ slide.location }}</p>
            </div>
          </div>
        </div>
      </motion.div>
      
      <!-- Left arrow navigation -->
      <button 
        @click="moveSlider(-1)"
        class="absolute left-4 z-10 text-white/70 hover:text-white transition-colors duration-300"
      >
        <Icon name="heroicons:chevron-left" class="w-10 h-10" />
      </button>
      
      <!-- Right arrow navigation -->
      <button 
        @click="moveSlider(1)"
        class="absolute right-4 z-10 text-white/70 hover:text-white transition-colors duration-300"
      >
        <Icon name="heroicons:chevron-right" class="w-10 h-10" />
      </button>
    </div>
    
    <!-- Selected model details overlay -->
    <motion.div
      v-if="currentSlide !== null"
      class="absolute bottom-0 left-0 right-0 bg-black/50 backdrop-blur-sm p-8 z-20"
      :initial="{ y: 100, opacity: 0 }"
      :animate="{ y: 0, opacity: 1 }"
      :transition="{ duration: 0.5 }"
    >
      <div class="flex justify-between items-center">
        <div class="max-w-2xl">
          <h2 class="text-4xl font-light tracking-wider text-white mb-2">{{ slides[currentSlide].title }}</h2>
          <p class="text-lg text-white/80">{{ slides[currentSlide].description }}</p>
        </div>
        <UButton color="white" variant="outline">View Portfolio</UButton>
      </div>
    </motion.div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { motion } from 'motion-v'

// Slides data - can be replaced with real data later
const slidesData = [
  { 
    id: 1, 
    alt: 'Model 1',
    modelName: 'Sophia Lee',
    location: 'New York',
    title: 'Discover New Talent',
    description: 'EliteModels represents extraordinary talents from around the world, bringing fresh faces to international fashion runways.'
  },
  { 
    id: 2, 
    alt: 'Model 2',
    modelName: 'James Wilson',
    location: 'Paris',
    title: 'Fashion Forward',
    description: 'Our models work with the most prestigious fashion houses and photographers, setting trends and defining the future of fashion.'
  },
  { 
    id: 3, 
    alt: 'Model 3',
    modelName: 'Elena Rodriguez',
    location: 'Milan',
    title: 'Join Our Agency',
    description: "We're always looking for new faces. Apply today to be represented by one of the most respected agencies in the industry."
  },
  { 
    id: 4, 
    alt: 'Model 4',
    modelName: 'David Chen',
    location: 'Tokyo',
    title: 'Global Presence',
    description: "Our network spans major fashion capitals worldwide, creating opportunities for models at every career stage."
  },
  { 
    id: 5, 
    alt: 'Model 5',
    modelName: 'Amara Okafor',
    location: 'London',
    title: 'Diverse Talent',
    description: "We celebrate diversity and inclusion, representing models from all backgrounds across commercial, editorial, and runway work."
  }
]

const slides = ref(slidesData)
const currentSlide = ref(0)
const containerRef = ref(null)
const isAnimating = ref(false)
const containerX = ref(0)
const slideWidth = ref(0)

// Touch handling variables
const touchStartX = ref(0)
const touchMoveX = ref(0)
const touchThreshold = 50 // Min swipe distance to trigger slide change

// Monitor current position to handle infinite scroll if needed
const currentPosition = ref(0)

// Calculate visible slide index based on position
const setCurrentSlide = (index) => {
  currentSlide.value = index
}

const moveSlider = (direction) => {
  if (isAnimating.value) return
  
  isAnimating.value = true
  setTimeout(() => { isAnimating.value = false }, 600)
  
  // Calculate new position
  const newPosition = currentPosition.value + direction
  
  // Check boundaries
  if (newPosition >= 0 && newPosition < slides.value.length) {
    currentPosition.value = newPosition
    
    // Move the container
    containerX.value -= direction * slideWidth.value
    
    // Update current slide
    currentSlide.value = Math.min(Math.max(currentSlide.value + direction, 0), slides.value.length - 1)
  }
}

const handleWheel = (event) => {
  if (isAnimating.value) return
  
  isAnimating.value = true
  setTimeout(() => { isAnimating.value = false }, 500)
  
  // Determine scroll direction (positive means scrolling right/down)
  const delta = event.deltaX !== 0 ? event.deltaX : event.deltaY
  const direction = delta > 0 ? 1 : -1
  
  moveSlider(direction)
}

// Touch event handlers for mobile support
const handleTouchStart = (event) => {
  touchStartX.value = event.touches[0].clientX
}

const handleTouchMove = (event) => {
  touchMoveX.value = event.touches[0].clientX
  
  // Prevent default to stop page scrolling while swiping
  event.preventDefault()
}

const handleTouchEnd = () => {
  if (isAnimating.value) return
  
  const touchDiff = touchStartX.value - touchMoveX.value
  
  // If the swipe was long enough
  if (Math.abs(touchDiff) > touchThreshold) {
    // Direction: positive means swipe left (show next), negative means swipe right (show prev)
    const direction = touchDiff > 0 ? 1 : -1
    moveSlider(direction)
  }
  
  // Reset touch values
  touchStartX.value = 0
  touchMoveX.value = 0
}

onMounted(() => {
  // Calculate approximate slide width based on window width
  slideWidth.value = window.innerWidth * 0.7 // Adjust based on your needs
  
  // Apply resize listener for responsive behavior
  window.addEventListener('resize', () => {
    slideWidth.value = window.innerWidth * 0.7
    
    // Recalculate position to ensure proper alignment after resize
    containerX.value = -currentPosition.value * slideWidth.value
  })
  
  // Center the first slide
  containerX.value = 0
})
</script>

<style scoped>
/* Smooth transitions for all animations */
.flex-row > div {
  transition: transform 0.5s ease-out;
}

/* Horizontal scrollbar hiding */
::-webkit-scrollbar {
  display: none;
}
</style> 