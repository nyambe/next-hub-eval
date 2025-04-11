<script setup lang="ts">
useSeoMeta({
  // Basic SEO
  title: 'ELITE MODELS | International Model Agency',
  description: 'ELITE MODELS represents extraordinary talents from around the world, bringing fresh faces to international fashion runways, campaigns, and editorial.',
  keywords: 'model agency, fashion models, modeling, high fashion, runway models, photoshoot, elite models, talent management',
  
  // Open Graph / Facebook
  ogTitle: 'ELITE MODELS | International Model Agency',
  ogDescription: 'Discover new talents and established models represented by one of the most prestigious agencies in the fashion industry.',
  ogImage: '/img/sample.png',
  ogUrl: 'https://elitemodels.example.com',
  ogType: 'website',
  
  // Twitter
  twitterTitle: 'ELITE MODELS | International Model Agency',
  twitterDescription: 'Representing extraordinary talents from around the world at ELITE MODELS.',
  twitterImage: '/img/sample.png',
  twitterCard: 'summary_large_image',
  
  // Additional Information
  themeColor: '#000000',
  colorScheme: 'dark',
  author: 'ELITE MODELS',
  robots: 'index, follow'
})

// Set dark theme by default
useHead({
  htmlAttrs: {
    lang: 'en'
  },
  link: [
    { rel: 'icon', type: 'image/png', href: '/favicon.ico' }
  ]
})
</script>

<template>
  <UApp>
    <NuxtPage />
  </UApp>
</template>