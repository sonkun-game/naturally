<template>
  <section id="shop" class="py-24 bg-white">
    <div class="max-w-7xl mx-auto px-6 lg:px-12">
      <!-- Split Header Layout -->
      <div class="flex flex-col md:flex-row md:items-end justify-between mb-16 gap-6">
        <div>
          <h2 class="font-serif text-3xl md:text-4xl lg:text-5xl font-bold text-naturally-dark mb-3">
            Our Bestsellers
          </h2>
          <p class="font-sans text-gray-600 text-lg">
            Formulated with pure botanical extracts for powerful, visible results.
          </p>
        </div>
        <a
          href="#shop"
          class="font-medium text-naturally-dark hover:text-naturally-green flex items-center gap-2 group transition-colors self-start md:self-auto text-base"
        >
          Shop All Products
          <ArrowRight class="w-5 h-5 transition-transform duration-300 group-hover:translate-x-1" />
        </a>
      </div>

      <!-- 4 Columns Product Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
        <div
          v-for="product in products"
          :key="product.id"
          class="group cursor-pointer flex flex-col"
        >
          <!-- Image Wrapper with aspect 4/5 -->
          <div class="relative aspect-[4/5] rounded-2xl bg-naturally-light overflow-hidden mb-4 shadow-sm">
            <img
              :src="product.image"
              :alt="product.name"
              class="w-full h-full object-cover transition-transform duration-700 ease-out group-hover:scale-105"
            />

            <!-- Badge (if organic/new) -->
            <span
              v-if="product.badge"
              class="absolute top-3 left-3 bg-white/90 backdrop-blur-md text-naturally-dark text-xs font-semibold px-3 py-1 rounded-full shadow-sm"
            >
              {{ product.badge }}
            </span>

            <!-- Slide-up Add to Cart Button on Hover -->
            <div class="absolute inset-x-4 bottom-4 translate-y-4 opacity-0 group-hover:translate-y-0 group-hover:opacity-100 transition-all duration-300 ease-out">
              <button
                @click.stop="onAddToCart(product)"
                class="w-full py-3 px-4 bg-white/95 backdrop-blur-md text-naturally-dark font-medium rounded-xl shadow-lg hover:bg-naturally-green hover:text-white transition-colors duration-200 flex items-center justify-center gap-2"
              >
                <Plus class="w-4 h-4" />
                Add to Cart
              </button>
            </div>
          </div>

          <!-- Product Details -->
          <div class="flex flex-col">
            <h3 class="font-sans font-semibold text-gray-900 group-hover:text-naturally-green transition-colors text-lg mb-1">
              {{ product.name }}
            </h3>
            <span class="font-serif text-naturally-dark font-bold text-lg">
              ${{ product.price.toFixed(2) }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ArrowRight, Plus } from 'lucide-vue-next'

const emit = defineEmits(['add-to-cart'])

interface Product {
  id: number
  name: string
  price: number
  image: string
  badge?: string
}

const products: Product[] = [
  {
    id: 1,
    name: 'Botanical Face Serum',
    price: 48.00,
    image: 'https://images.unsplash.com/photo-1620916566398-39f1143ab7be?q=80&w=800&auto=format&fit=crop',
    badge: 'Bestseller'
  },
  {
    id: 2,
    name: 'Hydrating Body Lotion',
    price: 36.00,
    image: 'https://images.unsplash.com/photo-1556228578-0d85b1a4d571?q=80&w=800&auto=format&fit=crop',
    badge: 'Organic'
  },
  {
    id: 3,
    name: 'Detoxifying Clay Mask',
    price: 42.00,
    image: 'https://images.unsplash.com/photo-1608248543803-ba4f8c70ae0b?q=80&w=800&auto=format&fit=crop',
    badge: 'New'
  },
  {
    id: 4,
    name: 'Exfoliating Scrub',
    price: 34.00,
    image: 'https://images.unsplash.com/photo-1599305090598-fe179d501227?q=80&w=800&auto=format&fit=crop'
  }
]

const onAddToCart = (product: Product) => {
  emit('add-to-cart', product)
}
</script>
