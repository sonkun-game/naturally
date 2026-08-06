<template>
  <section id="categories" class="py-24 bg-[#E8EFE5]">
    <div class="max-w-7xl mx-auto px-6 lg:px-12">
      <!-- Section Header -->
      <div class="text-center max-w-2xl mx-auto mb-16">
        <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-naturally-dark mb-4">
          Shop by Category
        </h2>
        <p class="font-sans text-gray-700 text-lg leading-relaxed">
          Find exactly what your body needs with our specialized natural collections.
        </p>
      </div>

      <!-- Categories 3-Column Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <a
          v-for="category in categories"
          :key="category.title"
          :href="category.href"
          class="group relative aspect-[4/5] rounded-2xl overflow-hidden shadow-md cursor-pointer block"
        >
          <!-- Background Image -->
          <img
            :src="category.image"
            :alt="category.title"
            class="w-full h-full object-cover transition-transform duration-700 ease-out group-hover:scale-105"
          />

          <!-- Dark Overlay -->
          <div
            class="absolute inset-0 bg-black/20 group-hover:bg-black/40 transition-colors duration-300 flex items-center justify-center p-6 text-center"
          >
            <h3 class="font-serif text-3xl text-white font-semibold tracking-wide drop-shadow-md">
              {{ category.title }}
            </h3>
          </div>
        </a>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
const categories = [
  {
    title: 'Face Care',
    image: 'https://images.unsplash.com/photo-1598440947619-2c35fc9aa908?q=80&w=800&auto=format&fit=crop',
    href: '#shop'
  },
  {
    title: 'Body Care',
    image: 'https://images.unsplash.com/photo-1617897903246-719242758050?q=80&w=800&auto=format&fit=crop',
    href: '#shop'
  },
  {
    title: 'Hair Care',
    image: 'https://images.unsplash.com/photo-1527799820374-dcf8d9d4a388?q=80&w=800&auto=format&fit=crop',
    href: '#shop'
  }
]
</script>
