<template>
  <!-- About Section with scroll animations -->
  <div class="min-h-screen bg-black relative overflow-hidden" id="about-section">
    <!-- Background gradient -->
    <div class="absolute inset-0 bg-gradient-to-b from-black via-gray-900 to-black"></div>
    
    <div class="container mx-auto px-4 py-24 relative z-10">
      <!-- About Us Section with motion animations -->
      <motion.div
        :initial="{ opacity: 0, y: 100 }"
        :whileInView="{ opacity: 1, y: 0 }"
        :transition="{ duration: 0.8, delay: 0.2 }"
        :viewport="{ once: true, margin: '-100px' }"
        class="max-w-4xl mx-auto text-center mb-20"
      >
        <h2 class="text-5xl font-light tracking-wider text-white mb-6">About <span class="font-bold">ELITE MODELS</span></h2>
        <p class="text-xl text-white/80 leading-relaxed">
          Founded in 2010, ELITE MODELS has established itself as one of the most prestigious model agencies in the world.
          With offices in New York, Paris, Milan, London, and Tokyo, we represent a diverse roster of talents.
        </p>
      </motion.div>
      
      <!-- Services Grid with staggered animations -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-6xl mx-auto">
        <motion.div
          v-for="(service, index) in services"
          :key="service.title"
          :initial="{ opacity: 0, y: 50 }"
          :whileInView="{ opacity: 1, y: 0 }"
          :transition="{ duration: 0.5, delay: 0.1 * index }"
          :viewport="{ once: true, margin: '-100px' }"
          class="bg-gray-900/50 p-8 rounded-xl backdrop-blur-sm"
        >
          <div class="text-white/90 mb-4 text-4xl">
            <Icon :name="service.icon" class="w-12 h-12" />
          </div>
          <h3 class="text-2xl font-medium text-white mb-3">{{ service.title }}</h3>
          <p class="text-white/70">{{ service.description }}</p>
        </motion.div>
      </div>
      
      <!-- CTA Button -->
      <motion.div
        :initial="{ opacity: 0, scale: 0.9 }"
        :whileInView="{ opacity: 1, scale: 1 }"
        :transition="{ duration: 0.5, delay: 0.6 }"
        :viewport="{ once: true }"
        class="mt-16 text-center"
      >
        <UButton size="lg" color="white" variant="outline">
          Discover Our Services
        </UButton>
      </motion.div>
    </div>
  </div>
</template>

<script setup>
import { motion } from 'motion-v'

// Services data for the About section
const services = [
  {
    title: 'Model Management',
    description: 'Full-service representation for models including career planning, booking management, and personalized development.',
    icon: 'heroicons:user-group'
  },
  {
    title: 'Talent Scouting',
    description: 'Discovering fresh faces and developing new talent through our global scouting network and open casting calls.',
    icon: 'heroicons:magnifying-glass'
  },
  {
    title: 'Production Services',
    description: 'End-to-end production capabilities for fashion shoots, campaigns, and runway shows for global brands.',
    icon: 'heroicons:camera'
  }
]
</script>

<style scoped>
/* Any additional styling specific to AboutSection */
</style> 