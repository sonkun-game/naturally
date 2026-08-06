<template>
  <header
    :class="[
      'fixed top-0 w-full z-50 transition-all duration-300',
      isScrolled ? 'bg-white/80 backdrop-blur-md shadow-sm py-4' : 'bg-transparent py-6'
    ]"
  >
    <div class="max-w-7xl mx-auto px-6 lg:px-12 flex justify-between items-center">
      <!-- Left: Logo -->
      <a href="#" class="flex items-center gap-2 group cursor-pointer text-decoration-none">
        <Leaf
          class="w-7 h-7 text-naturally-green transition-transform duration-300 group-hover:rotate-12"
        />
        <span class="font-serif text-2xl tracking-wide text-naturally-dark font-semibold">
          Naturally
        </span>
      </a>

      <!-- Center: Desktop Navigation Links -->
      <nav class="hidden md:flex items-center space-x-8">
        <a
          v-for="link in navLinks"
          :key="link.name"
          :href="link.href"
          class="font-medium text-sm lg:text-base text-gray-700 hover:text-naturally-green transition-colors duration-200"
        >
          {{ link.name }}
        </a>
      </nav>

      <!-- Right: Action Icons -->
      <div class="flex items-center space-x-5">
        <!-- Search Button -->
        <button
          @click="isSearchOpen = true"
          aria-label="Search"
          class="text-gray-700 hover:text-naturally-green transition-colors p-1.5 rounded-full hover:bg-naturally-light"
        >
          <Search class="w-5 h-5" />
        </button>

        <!-- User Profile Button -->
        <button
          @click="isUserOpen = true"
          aria-label="User account"
          class="text-gray-700 hover:text-naturally-green transition-colors p-1.5 rounded-full hover:bg-naturally-light"
        >
          <User class="w-5 h-5" />
        </button>

        <!-- Cart Button with Green Notification Badge -->
        <button
          @click="isCartOpen = true"
          aria-label="Shopping Cart"
          class="relative text-gray-700 hover:text-naturally-green transition-colors p-1.5 rounded-full hover:bg-naturally-light"
        >
          <ShoppingCart class="w-5 h-5" />
          <span
            v-if="cartCount > 0"
            class="absolute -top-1 -right-1 bg-naturally-green text-white text-[11px] font-bold w-5 h-5 rounded-full flex items-center justify-center border-2 border-white shadow-sm"
          >
            {{ cartCount }}
          </span>
        </button>

        <!-- Mobile Menu Toggle Button -->
        <button
          @click="isMobileMenuOpen = !isMobileMenuOpen"
          aria-label="Toggle Menu"
          class="md:hidden text-gray-700 hover:text-naturally-green p-1.5 rounded-lg"
        >
          <Menu v-if="!isMobileMenuOpen" class="w-6 h-6" />
          <X v-else class="w-6 h-6" />
        </button>
      </div>
    </div>

    <!-- Mobile Dropdown Menu -->
    <Transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="opacity-0 -translate-y-4"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100 translate-y-0"
      leave-to-class="opacity-0 -translate-y-4"
    >
      <div
        v-if="isMobileMenuOpen"
        class="md:hidden bg-white/95 backdrop-blur-md border-b border-gray-100 px-6 py-6 shadow-lg"
      >
        <div class="flex flex-col space-y-4">
          <a
            v-for="link in navLinks"
            :key="link.name"
            :href="link.href"
            @click="isMobileMenuOpen = false"
            class="font-medium text-lg text-gray-800 hover:text-naturally-green transition-colors"
          >
            {{ link.name }}
          </a>
        </div>
      </div>
    </Transition>

    <!-- Search Modal -->
    <Transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100 scale-100"
      leave-to-class="opacity-0 scale-95"
    >
      <div
        v-if="isSearchOpen"
        class="fixed inset-0 z-50 bg-black/50 backdrop-blur-sm flex items-start justify-center pt-24 px-4"
        @click.self="isSearchOpen = false"
      >
        <div class="bg-white rounded-2xl p-6 w-full max-w-xl shadow-2xl relative">
          <button
            @click="isSearchOpen = false"
            class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 p-1"
          >
            <X class="w-5 h-5" />
          </button>
          <h3 class="font-serif text-xl text-naturally-dark font-semibold mb-4">Search Products</h3>
          <div class="relative">
            <Search class="w-5 h-5 text-gray-400 absolute left-4 top-1/2 -translate-y-1/2" />
            <input
              type="text"
              v-model="searchQuery"
              placeholder="Search organic serums, moisturizers, masks..."
              class="w-full pl-12 pr-4 py-3 bg-naturally-light rounded-xl border-none focus:outline-none focus:ring-2 focus:ring-naturally-green text-sm"
              autofocus
            />
          </div>
          <div class="mt-4 flex flex-wrap gap-2 text-xs text-gray-500">
            <span>Popular:</span>
            <button @click="searchQuery = 'Serum'" class="bg-gray-100 hover:bg-naturally-light px-2.5 py-1 rounded-full text-naturally-dark">Face Serum</button>
            <button @click="searchQuery = 'Cream'" class="bg-gray-100 hover:bg-naturally-light px-2.5 py-1 rounded-full text-naturally-dark">Clay Mask</button>
            <button @click="searchQuery = 'Organic'" class="bg-gray-100 hover:bg-naturally-light px-2.5 py-1 rounded-full text-naturally-dark">Organic Body Lotion</button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Slide-over Cart Drawer -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="translate-x-full"
      enter-to-class="translate-x-0"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="translate-x-0"
      leave-to-class="translate-x-full"
    >
      <div
        v-if="isCartOpen"
        class="fixed inset-y-0 right-0 z-50 w-full max-w-md bg-white shadow-2xl flex flex-col justify-between"
      >
        <div class="p-6 border-b border-gray-100 flex items-center justify-between">
          <div class="flex items-center gap-2">
            <ShoppingCart class="w-5 h-5 text-naturally-green" />
            <h3 class="font-serif text-xl font-semibold text-naturally-dark">Your Cart ({{ cartCount }})</h3>
          </div>
          <button @click="isCartOpen = false" class="text-gray-400 hover:text-gray-600 p-1">
            <X class="w-5 h-5" />
          </button>
        </div>

        <div class="p-6 overflow-y-auto flex-1 space-y-4">
          <div
            v-for="(item, index) in cartItems"
            :key="index"
            class="flex items-center gap-4 p-3 bg-naturally-light/60 rounded-xl"
          >
            <img :src="item.image" :alt="item.name" class="w-16 h-16 object-cover rounded-lg" />
            <div class="flex-1">
              <h4 class="font-medium text-sm text-naturally-dark">{{ item.name }}</h4>
              <p class="text-xs text-gray-500 font-sans mt-0.5">${{ item.price.toFixed(2) }}</p>
            </div>
            <button @click="removeItem(index)" class="text-gray-400 hover:text-red-500 p-1">
              <Trash2 class="w-4 h-4" />
            </button>
          </div>

          <div v-if="cartItems.length === 0" class="text-center py-12 text-gray-500">
            <ShoppingCart class="w-12 h-12 mx-auto text-gray-300 mb-3" />
            <p>Your cart is empty.</p>
          </div>
        </div>

        <div class="p-6 border-t border-gray-100 bg-naturally-light/30">
          <div class="flex justify-between items-center mb-4">
            <span class="text-gray-600 font-medium">Subtotal</span>
            <span class="font-serif text-xl font-bold text-naturally-dark">${{ cartTotal.toFixed(2) }}</span>
          </div>
          <button
            @click="checkout"
            class="w-full py-3.5 bg-naturally-dark hover:bg-[#1a250e] text-white font-medium rounded-xl transition-colors shadow-sm"
          >
            Proceed to Checkout
          </button>
        </div>
      </div>
    </Transition>
  </header>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { Leaf, Search, User, ShoppingCart, Menu, X, Trash2 } from 'lucide-vue-next'

const props = defineProps({
  cartItemsProps: {
    type: Array as () => Array<{ id: number; name: string; price: number; image: string }>,
    default: () => [
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
    ]
  }
})

const emit = defineEmits(['update-cart'])

const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)
const isSearchOpen = ref(false)
const isCartOpen = ref(false)
const isUserOpen = ref(false)
const searchQuery = ref('')

const cartItems = ref([...props.cartItemsProps])

const navLinks = [
  { name: 'Shop', href: '#shop' },
  { name: 'Categories', href: '#categories' },
  { name: 'About Us', href: '#about' },
  { name: 'Sustainability', href: '#sustainability' },
  { name: 'Blog', href: '#blog' }
]

const cartCount = computed(() => cartItems.value.length)
const cartTotal = computed(() => cartItems.value.reduce((sum, item) => sum + item.price, 0))

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const removeItem = (index: number) => {
  cartItems.value.splice(index, 1)
  emit('update-cart', cartItems.value)
}

const checkout = () => {
  alert('Thank you for choosing Naturally! Redirecting to secure checkout...')
  isCartOpen.value = false
}

// Expose openCart for parent components to trigger
defineExpose({
  openCart: () => { isCartOpen.value = true },
  addItem: (item: { id: number; name: string; price: number; image: string }) => {
    cartItems.value.push(item)
    isCartOpen.value = true
  }
})

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>
