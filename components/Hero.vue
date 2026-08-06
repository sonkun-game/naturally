<template>
  <section class="min-h-screen relative flex items-center justify-center overflow-hidden pt-20">
    <!-- HTML5 Background Video without dark overlay -->
    <video
      autoplay
      muted
      loop
      playsinline
      class="absolute inset-0 w-full h-full object-cover z-0 opacity-80 pointer-events-none"
      poster="https://images.unsplash.com/photo-1540555700478-4be289fbecef?q=80&w=1600&auto=format&fit=crop"
    >
      <source src="https://assets.mixkit.co/videos/preview/mixkit-drops-of-water-falling-on-a-green-leaf-41618-large.mp4" type="video/mp4" />
      <source src="https://assets.mixkit.co/videos/preview/mixkit-close-up-of-green-leaves-4089-large.mp4" type="video/mp4" />
    </video>

    <!-- Subtle gradient to blend top header softly -->
    <div class="absolute top-0 inset-x-0 h-32 bg-gradient-to-b from-white/40 to-transparent z-10 pointer-events-none"></div>

    <!-- Centered Hero Content -->
    <div class="relative z-20 max-w-4xl mx-auto px-6 text-center py-20 flex flex-col items-center">
      <!-- Tagline Badge -->
      <div class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full bg-white/70 backdrop-blur-sm border border-naturally-green/20 mb-6 text-xs font-semibold uppercase tracking-wider text-naturally-dark shadow-sm">
        <span class="w-2 h-2 rounded-full bg-naturally-green animate-pulse"></span>
        100% Certified Organic & Vegan
      </div>

      <!-- Headline -->
      <h1 class="font-serif text-5xl md:text-6xl lg:text-7xl font-bold text-naturally-dark leading-tight tracking-tight mb-6">
        Pure, Natural Beauty.
      </h1>

      <!-- Subheadline -->
      <p class="font-sans text-lg md:text-xl text-gray-800 max-w-2xl leading-relaxed mb-10">
        Discover our new collection of sustainable, organic skincare products designed to enhance your natural glow.
      </p>

      <!-- Buttons -->
      <div class="flex flex-col sm:flex-row items-center justify-center gap-5 w-full sm:w-auto">
        <!-- Primary Button -->
        <a
          href="#shop"
          class="w-full sm:w-auto px-8 py-4 bg-naturally-dark hover:bg-[#1a250e] text-white font-medium rounded-full shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-0.5 text-center"
        >
          Shop Collection
        </a>

        <!-- Secondary Button with expanding underline animation -->
        <a
          href="#about"
          class="w-full sm:w-auto px-8 py-4 text-naturally-dark font-medium underline-animation transition-colors text-center inline-block"
        >
          Our Story
        </a>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
</script>
