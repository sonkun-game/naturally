<template>
  <section class="py-24 bg-naturally-dark text-white relative overflow-hidden">
    <!-- Ambient subtle background glow -->
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-naturally-green/10 rounded-full blur-3xl pointer-events-none"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6 lg:px-12">
      <!-- Centered Section Header -->
      <div class="text-center max-w-2xl mx-auto mb-16">
        <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-white mb-4">
          Loved by our Community
        </h2>
        <p class="font-sans text-gray-300 text-lg leading-relaxed">
          See how Naturally transformed skincare routines around the world.
        </p>
      </div>

      <!-- 3 Columns Glassmorphic Cards Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div
          v-for="testimonial in testimonials"
          :key="testimonial.author"
          class="bg-white/5 backdrop-blur-sm rounded-2xl p-8 border border-white/10 flex flex-col justify-between hover:bg-white/10 transition-colors duration-300 shadow-xl"
        >
          <div>
            <!-- 5-Star Rating Icons -->
            <div class="flex items-center gap-1 mb-6">
              <Star
                v-for="i in 5"
                :key="i"
                class="w-5 h-5 text-naturally-green fill-naturally-green"
              />
            </div>

            <!-- Quote Text -->
            <p class="font-sans text-gray-200 text-base md:text-lg leading-relaxed italic mb-8">
              "{{ testimonial.quote }}"
            </p>
          </div>

          <!-- Author Info -->
          <div class="flex items-center gap-4 pt-4 border-t border-white/10">
            <img
              :src="testimonial.avatar"
              :alt="testimonial.author"
              class="w-12 h-12 rounded-full object-cover border-2 border-naturally-green/40"
            />
            <div>
              <h4 class="font-serif font-bold text-white text-lg">
                {{ testimonial.author }}
              </h4>
              <span class="text-sm text-gray-400 font-sans block">
                {{ testimonial.title }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { Star } from 'lucide-vue-next'

const testimonials = [
  {
    quote: "My sensitive skin has never looked cleaner or felt softer. The Botanical Serum is now my holy grail morning product!",
    author: "Elena Rostova",
    title: "Verified Customer • New York",
    avatar: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=200&auto=format&fit=crop"
  },
  {
    quote: "Knowing every jar is sustainably sourced and zero plastic makes me love Naturally even more. Exceptional quality.",
    author: "Sophia Chen",
    title: "Eco Blogger • San Francisco",
    avatar: "https://images.unsplash.com/photo-1517841905240-472988babdf9?q=80&w=200&auto=format&fit=crop"
  },
  {
    quote: "The Detoxifying Clay Mask noticeably refined my pores after just two uses. Highly recommend for any natural routine.",
    author: "Marcus Vance",
    title: "Verified Customer • London",
    avatar: "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?q=80&w=200&auto=format&fit=crop"
  }
]
</script>
