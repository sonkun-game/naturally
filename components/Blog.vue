<template>
  <section id="blog" class="py-24 bg-white">
    <div class="max-w-7xl mx-auto px-6 lg:px-12">
      <!-- Split Header Layout -->
      <div class="flex flex-col md:flex-row md:items-end justify-between mb-16 gap-6">
        <div>
          <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-naturally-dark mb-3">
            The Natural Journal
          </h2>
          <p class="font-sans text-gray-600 text-lg">
            Insights, guides, and rituals for sustainable holistic skincare.
          </p>
        </div>
        <a
          href="#blog"
          class="font-medium text-naturally-dark hover:text-naturally-green flex items-center gap-2 group transition-colors self-start md:self-auto text-base"
        >
          Read the Blog
          <ArrowRight class="w-5 h-5 transition-transform duration-300 group-hover:translate-x-1" />
        </a>
      </div>

      <!-- 3 Columns Blog Cards Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <article
          v-for="post in posts"
          :key="post.id"
          class="group cursor-pointer flex flex-col"
        >
          <!-- Image Container aspect 3/2 -->
          <div class="relative aspect-[3/2] rounded-2xl overflow-hidden mb-5 bg-naturally-light shadow-sm">
            <img
              :src="post.image"
              :alt="post.title"
              class="w-full h-full object-cover transition-transform duration-700 ease-out group-hover:scale-105"
            />
            <!-- Top-Left Glass Badge -->
            <span class="absolute top-4 left-4 bg-white/90 backdrop-blur-sm text-naturally-dark font-semibold text-xs px-3 py-1 rounded-full shadow-sm">
              {{ post.badge }}
            </span>
          </div>

          <!-- Metadata & Title -->
          <span class="text-sm text-gray-500 font-sans mb-2">
            {{ post.date }} • {{ post.readTime }}
          </span>

          <h3 class="font-serif text-xl font-bold text-naturally-dark group-hover:text-naturally-green transition-colors leading-snug">
            {{ post.title }}
          </h3>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ArrowRight } from 'lucide-vue-next'

const posts = [
  {
    id: 1,
    title: 'The Art of Cold-Pressed Botanical Oils in Skincare',
    date: 'August 4, 2026',
    readTime: '4 min read',
    badge: 'Education',
    image: 'https://images.unsplash.com/photo-1556228453-efd6c1ff04f6?q=80&w=800&auto=format&fit=crop'
  },
  {
    id: 2,
    title: '5 Daily Habits for a Radiant Sustainable Glow',
    date: 'July 28, 2026',
    readTime: '6 min read',
    badge: 'Lifestyle',
    image: 'https://images.unsplash.com/photo-1556228578-0d85b1a4d571?q=80&w=800&auto=format&fit=crop'
  },
  {
    id: 3,
    title: 'Understanding Natural Minerals and Earth Clays',
    date: 'July 15, 2026',
    readTime: '5 min read',
    badge: 'Ingredients',
    image: 'https://images.unsplash.com/photo-1608248543803-ba4f8c70ae0b?q=80&w=800&auto=format&fit=crop'
  }
]
</script>
