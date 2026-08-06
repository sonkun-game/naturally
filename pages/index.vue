<template>
  <div class="relative min-h-screen bg-white font-sans selection:bg-naturally-green selection:text-white">
    <!-- Navigation Bar -->
    <Navbar ref="navbarRef" :cart-items-props="cartItems" @update-cart="onUpdateCart" />

    <!-- Main Content Sections -->
    <main>
      <!-- Hero Section -->
      <Hero />

      <!-- Categories Section -->
      <Categories />

      <!-- Products / Bestsellers Section -->
      <Products @add-to-cart="handleAddToCart" />

      <!-- Sustainability / Features Section -->
      <Features />

      <!-- About Us Section -->
      <About />

      <!-- Testimonials Section -->
      <Testimonials />

      <!-- Natural Journal / Blog Section -->
      <Blog />
    </main>

    <!-- Footer -->
    <Footer />

    <!-- Toast Notification for Add to Cart -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="translate-y-10 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-10 opacity-0"
    >
      <div
        v-if="toastMessage"
        class="fixed bottom-6 right-6 z-50 bg-naturally-dark text-white px-5 py-3.5 rounded-2xl shadow-2xl flex items-center gap-3 border border-white/20"
      >
        <div class="w-8 h-8 rounded-full bg-naturally-green/20 text-naturally-green flex items-center justify-center">
          <CheckCircle class="w-5 h-5 text-naturally-green" />
        </div>
        <div>
          <h5 class="font-medium text-sm text-white font-serif">Added to Cart!</h5>
          <p class="text-xs text-gray-300 font-sans">{{ toastMessage }}</p>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { CheckCircle } from 'lucide-vue-next'
import Navbar from '~/components/Navbar.vue'
import Hero from '~/components/Hero.vue'
import Categories from '~/components/Categories.vue'
import Products from '~/components/Products.vue'
import Features from '~/components/Features.vue'
import About from '~/components/About.vue'
import Testimonials from '~/components/Testimonials.vue'
import Blog from '~/components/Blog.vue'
import Footer from '~/components/Footer.vue'

interface Product {
  id: number
  name: string
  price: number
  image: string
  badge?: string
}

const navbarRef = ref<any>(null)
const toastMessage = ref('')

const cartItems = ref<Product[]>([
  {
    id: 1,
    name: 'Botanical Face Serum',
    price: 48.00,
    image: 'https://images.unsplash.com/photo-1620916566398-39f1143ab7be?q=80&w=800&auto=format&fit=crop'
  },
  {
    id: 2,
    name: 'Hydrating Body Lotion',
    price: 36.00,
    image: 'https://images.unsplash.com/photo-1556228578-0d85b1a4d571?q=80&w=800&auto=format&fit=crop'
  }
])

const handleAddToCart = (product: Product) => {
  if (navbarRef.value) {
    navbarRef.value.addItem(product)
  } else {
    cartItems.value.push(product)
  }
  
  toastMessage.value = `${product.name} has been added.`
  setTimeout(() => {
    toastMessage.value = ''
  }, 3000)
}

const onUpdateCart = (newCart: Product[]) => {
  cartItems.value = newCart
}
</script>
