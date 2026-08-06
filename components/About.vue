<template>
  <section id="about" class="py-24 bg-white">
    <div class="max-w-7xl mx-auto px-6 lg:px-12">
      <div class="flex flex-col lg:flex-row items-center gap-12 lg:gap-16">
        <!-- Left Side: Image with aspect 4/5 & green accent overlay -->
        <div class="w-full lg:w-1/2">
          <div class="relative aspect-[4/5] rounded-2xl overflow-hidden shadow-xl group">
            <img
              src="https://images.unsplash.com/photo-1515377905703-c4788e51af15?q=80&w=800&auto=format&fit=crop"
              alt="Rooted in Nature"
              class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
            />
            <!-- Subtle bg-naturally-green/10 overlay -->
            <div class="absolute inset-0 bg-naturally-green/10 pointer-events-none"></div>

            <!-- Floating Badge -->
            <div class="absolute bottom-6 left-6 bg-white/90 backdrop-blur-md p-4 rounded-xl shadow-lg border border-white max-w-xs">
              <span class="font-serif text-2xl font-bold text-naturally-dark block mb-1">100%</span>
              <span class="text-xs text-gray-600 font-sans uppercase tracking-wider font-semibold">Pure Botanical Actives</span>
            </div>
          </div>
        </div>

        <!-- Right Side: Content -->
        <div class="w-full lg:w-1/2 flex flex-col items-start">
          <span class="text-naturally-green font-semibold uppercase tracking-wider text-sm mb-3">Our Mission</span>
          <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-naturally-dark mb-6 leading-tight">
            Rooted in Nature. Backed by Science.
          </h2>

          <p class="font-sans text-gray-600 text-lg leading-relaxed mb-4">
            Founded on the belief that true beauty comes from clean, uncompromised ingredients, Naturally creates high-performance skincare formulas that honor both your skin and the earth.
          </p>

          <p class="font-sans text-gray-600 text-lg leading-relaxed mb-8">
            We partner directly with sustainable organic farms worldwide to harvest nutrient-rich botanicals at peak potency, combining ancient herbal wisdom with modern dermatological innovation.
          </p>

          <button
            @click="readStory"
            class="px-8 py-4 bg-naturally-dark hover:bg-[#1a250e] text-white font-medium rounded-full shadow-md hover:shadow-lg transition-all duration-300 transform hover:-translate-y-0.5"
          >
            Read Our Full Story
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
const readStory = () => {
  alert('Welcome to Naturally! Our story began with a commitment to pure botanical skincare.')
}
</script>
