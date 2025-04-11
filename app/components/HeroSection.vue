<template>
  <div class="relative h-screen overflow-hidden bg-black">
    <!-- Top gradient overlay for navbar -->
    <div class="absolute top-0 left-0 right-0 h-32 z-10 bg-gradient-to-b from-black/80 to-transparent pointer-events-none"></div>
    
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
    
    <!-- Navigation Menu - Desktop -->
    <div class="absolute top-8 right-8 z-20 hidden md:block">
      <nav class="flex items-center gap-8">
        <a href="#" class="text-white hover:text-white/80 text-sm uppercase tracking-wider transition-colors">Models</a>
        <a href="#" class="text-white hover:text-white/80 text-sm uppercase tracking-wider transition-colors">About</a>
        <a href="#" class="text-white hover:text-white/80 text-sm uppercase tracking-wider transition-colors">Services</a>
        <a href="#" class="text-white hover:text-white/80 text-sm uppercase tracking-wider transition-colors">Contact</a>
      </nav>
    </div>
    
    <!-- Mobile Menu Toggle -->
    <div class="absolute top-8 right-8 z-30 md:hidden">
      <button 
        @click="isMenuOpen = !isMenuOpen" 
        class="text-white p-2"
      >
        <Icon v-if="!isMenuOpen" name="heroicons:bars-3" class="w-6 h-6" />
        <Icon v-else name="heroicons:x-mark" class="w-6 h-6" />
      </button>
    </div>
    
    <!-- Mobile Menu Panel -->
    <motion.div
      v-if="isMenuOpen"
      class="fixed inset-0 bg-black/95 z-20 md:hidden"
      :initial="{ opacity: 0 }"
      :animate="{ opacity: 1 }"
      :transition="{ duration: 0.3 }"
    >
      <div class="flex flex-col items-center justify-center h-full">
        <nav class="flex flex-col items-center gap-8">
          <a 
            v-for="(item, index) in navItems" 
            :key="index"
            href="#" 
            class="text-white hover:text-white/80 text-xl uppercase tracking-wider transition-colors"
            @click="isMenuOpen = false"
          >
            {{ item }}
          </a>
        </nav>
      </div>
    </motion.div>
    
    <!-- Ticker Container -->
    <div 
      class="relative h-screen flex items-center"
      @wheel.prevent="handleWheel"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
      @mouseover="pauseAutoScroll"
      @mouseleave="resumeAutoScroll"
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
              :src="`/img/${slide.image}`" 
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
        
        <!-- Clone of first few slides for seamless looping -->
        <div 
          v-for="(slide, index) in loopingSlides" 
          :key="`loop-${index}`"
          class="flex-shrink-0 h-screen w-auto relative"
          @click="handleLoopingSlideClick(index)"
        >
          <!-- Model image -->
          <div 
            class="h-full w-auto overflow-hidden relative"
          >
            <img 
              :src="`/img/${slide.image}`" 
              :alt="`Model ${index + 1}`" 
              class="h-full w-auto object-cover"
            />
            
            <!-- Hover/selected overlay -->
            <div 
              class="absolute inset-0 bg-black/30 transition-opacity duration-300 hover:opacity-30"
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
import { ref, onMounted, computed, onBeforeUnmount } from 'vue'
import { motion } from 'motion-v'

// Navigation items
const navItems = ['Models', 'About', 'Services', 'Contact']
const isMenuOpen = ref(false)

// Slides data - can be replaced with real data later
const slidesData = [
  { 
    id: 1, 
    image: 'sample.png',
    modelName: 'Sophia Lee',
    location: 'New York',
    title: 'Discover New Talent',
    description: 'EliteModels represents extraordinary talents from around the world, bringing fresh faces to international fashion runways.'
  },
  { 
    id: 2, 
    image: 'sample2.png',
    modelName: 'James Wilson',
    location: 'Paris',
    title: 'Fashion Forward',
    description: 'Our models work with the most prestigious fashion houses and photographers, setting trends and defining the future of fashion.'
  },
  { 
    id: 3, 
    image: 'sample3.png',
    modelName: 'Elena Rodriguez',
    location: 'Milan',
    title: 'Join Our Agency',
    description: "We're always looking for new faces. Apply today to be represented by one of the most respected agencies in the industry."
  },
  { 
    id: 4, 
    image: 'sample.png',
    modelName: 'David Chen',
    location: 'Tokyo',
    title: 'Global Presence',
    description: "Our network spans major fashion capitals worldwide, creating opportunities for models at every career stage."
  },
  { 
    id: 5, 
    image: 'sample2.png',
    modelName: 'Amara Okafor',
    location: 'London',
    title: 'Diverse Talent',
    description: "We celebrate diversity and inclusion, representing models from all backgrounds across commercial, editorial, and runway work."
  }
]

// Create looping slides that are copies of the first few slides
const loopingSlides = computed(() => {
  // Take the first 3 slides (or however many needed) for loop continuation
  return slidesData.slice(0, 3)
})

const slides = ref(slidesData)
const currentSlide = ref(0)
const containerRef = ref(null)
const isAnimating = ref(false)
const containerX = ref(0)
const slideWidth = ref(0)

// Auto-scrolling
const autoScrollTimer = ref(null)
const autoScrollEnabled = ref(true)
const autoScrollInterval = 3000 // 3 seconds

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

// Handle click on a looping slide (at the end)
const handleLoopingSlideClick = (index) => {
  // Reset to the start with the equivalent slide
  resetToStart()
  setCurrentSlide(index)
}

const moveSlider = (direction) => {
  if (isAnimating.value) return
  
  isAnimating.value = true
  setTimeout(() => { isAnimating.value = false }, 600)
  
  // Calculate new position
  const newPosition = currentPosition.value + direction
  
  // If we're at the end and moving forward, loop back to start
  if (newPosition >= slides.value.length) {
    resetToStart()
    return
  }
  
  // If we're at the start and moving backward, jump to end
  if (newPosition < 0) {
    jumpToEnd()
    return
  }
  
  // Normal movement within bounds
  currentPosition.value = newPosition
  containerX.value -= direction * slideWidth.value
  currentSlide.value = Math.min(Math.max(currentSlide.value + direction, 0), slides.value.length - 1)
}

// Reset to the start when we reach the end
const resetToStart = () => {
  // Animate to the first slide without animation (for looping)
  isAnimating.value = true
  setTimeout(() => {
    containerX.value = 0
    currentPosition.value = 0
    currentSlide.value = 0
    isAnimating.value = false
  }, 50)
}

// Jump to end when going backward from the start
const jumpToEnd = () => {
  isAnimating.value = true
  setTimeout(() => {
    const lastIndex = slides.value.length - 1
    containerX.value = -lastIndex * slideWidth.value
    currentPosition.value = lastIndex
    currentSlide.value = lastIndex
    isAnimating.value = false
  }, 50)
}

const handleWheel = (event) => {
  if (isAnimating.value) return
  
  // Stop auto-scrolling when user interacts
  pauseAutoScroll()
  
  isAnimating.value = true
  setTimeout(() => { isAnimating.value = false }, 500)
  
  // Determine scroll direction (positive means scrolling right/down)
  const delta = event.deltaX !== 0 ? event.deltaX : event.deltaY
  const direction = delta > 0 ? 1 : -1
  
  moveSlider(direction)
}

// Auto-scroll function
const startAutoScroll = () => {
  if (!autoScrollTimer.value && autoScrollEnabled.value) {
    autoScrollTimer.value = setInterval(() => {
      moveSlider(1) // Move to the next slide
    }, autoScrollInterval)
  }
}

const pauseAutoScroll = () => {
  if (autoScrollTimer.value) {
    clearInterval(autoScrollTimer.value)
    autoScrollTimer.value = null
    
    // Resume after 5 seconds of inactivity
    setTimeout(() => {
      if (autoScrollEnabled.value) {
        startAutoScroll()
      }
    }, 5000)
  }
}

const resumeAutoScroll = () => {
  if (autoScrollEnabled.value && !autoScrollTimer.value) {
    startAutoScroll()
  }
}

// Touch event handlers for mobile support
const handleTouchStart = (event) => {
  touchStartX.value = event.touches[0].clientX
  pauseAutoScroll()
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
  
  // Start auto-scrolling
  startAutoScroll()
})

onBeforeUnmount(() => {
  // Clean up timer when component is unmounted
  if (autoScrollTimer.value) {
    clearInterval(autoScrollTimer.value)
  }
  
  // Remove resize listener
  window.removeEventListener('resize', () => {})
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