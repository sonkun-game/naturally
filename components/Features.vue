<template>
  <section id="sustainability" class="py-24 bg-naturally-light">
    <div class="max-w-7xl mx-auto px-6 lg:px-12">
      <!-- Centered Section Header -->
      <div class="text-center max-w-2xl mx-auto mb-16">
        <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-naturally-dark mb-4">
          Nature's Best, For You
        </h2>
        <p class="font-sans text-gray-600 text-lg leading-relaxed">
          Our commitment to your skin health and the planet guides every formula we create.
        </p>
      </div>

      <!-- 3 Columns Features Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div
          v-for="feature in features"
          :key="feature.title"
          class="flex flex-col items-center text-center p-8 bg-white/60 backdrop-blur-sm rounded-2xl border border-white shadow-sm hover:shadow-md transition-shadow duration-300"
        >
          <!-- W-16 H-16 White Circle Icon Wrapper -->
          <div class="w-16 h-16 rounded-full bg-white flex items-center justify-center shadow-sm mb-6 text-naturally-green border border-gray-100">
            <component :is="feature.icon" class="w-8 h-8" />
          </div>

          <!-- Feature Title -->
          <h3 class="font-serif text-xl font-bold text-naturally-dark mb-3">
            {{ feature.title }}
          </h3>

          <!-- Feature Description -->
          <p class="font-sans text-gray-600 leading-relaxed text-base">
            {{ feature.description }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { Leaf, Recycle, ShieldCheck } from 'lucide-vue-next'

const features = [
  {
    title: '100% Organic',
    icon: Leaf,
    description: 'All products are certified organic, free from synthetic chemicals, parabens, and artificial fragrances.'
  },
  {
    title: 'Sustainably Sourced',
    icon: Recycle,
    description: 'Ethically harvested ingredients in recyclable, eco-friendly glass and biodegradable packaging.'
  },
  {
    title: 'Clinically Proven',
    icon: ShieldCheck,
    description: 'Dermatologist tested and proven to nourish, restore, and protect your skin naturally.'
  }
]
</script>
