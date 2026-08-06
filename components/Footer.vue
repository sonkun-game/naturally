<template>
  <footer class="bg-white border-t border-gray-200">
    <div class="max-w-7xl mx-auto px-6 lg:px-12 py-16">
      <!-- 4 Columns Desktop Grid -->
      <div class="grid grid-cols-1 md:grid-cols-4 gap-10 lg:gap-12 mb-12">
        <!-- Column 1: Brand Logo, Description, Raw SVG Social Media Icons -->
        <div class="flex flex-col space-y-4">
          <a href="#" class="flex items-center gap-2 group cursor-pointer text-decoration-none">
            <Leaf class="w-7 h-7 text-naturally-green transition-transform duration-300 group-hover:rotate-12" />
            <span class="font-serif text-2xl tracking-wide text-naturally-dark font-semibold">
              Naturally
            </span>
          </a>
          <p class="font-sans text-gray-600 text-sm leading-relaxed">
            Pure, sustainable, organic skincare crafted to nourish your skin and elevate your daily wellness rituals.
          </p>

          <!-- Raw SVG Social Icons -->
          <div class="flex items-center space-x-4 pt-2">
            <!-- Instagram -->
            <a
              href="https://instagram.com"
              target="_blank"
              rel="noopener noreferrer"
              aria-label="Instagram"
              class="text-gray-500 hover:text-naturally-green transition-colors p-1"
            >
              <svg class="w-5 h-5 fill-current" viewBox="0 0 24 24">
                <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
              </svg>
            </a>

            <!-- Twitter / X -->
            <a
              href="https://twitter.com"
              target="_blank"
              rel="noopener noreferrer"
              aria-label="Twitter"
              class="text-gray-500 hover:text-naturally-green transition-colors p-1"
            >
              <svg class="w-5 h-5 fill-current" viewBox="0 0 24 24">
                <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
              </svg>
            </a>

            <!-- Facebook -->
            <a
              href="https://facebook.com"
              target="_blank"
              rel="noopener noreferrer"
              aria-label="Facebook"
              class="text-gray-500 hover:text-naturally-green transition-colors p-1"
            >
              <svg class="w-5 h-5 fill-current" viewBox="0 0 24 24">
                <path d="M9 8H6v4h3v12h5V12h3.642L18 8h-4V6.333C14 5.374 14.5 5 15.5 5H18V0h-3.808C10.592 0 9 1.583 9 4.615V8z"/>
              </svg>
            </a>
          </div>
        </div>

        <!-- Column 2: Shop Links -->
        <div>
          <h4 class="font-serif font-bold text-naturally-dark text-base mb-4">
            Shop
          </h4>
          <ul class="space-y-2.5">
            <li v-for="link in shopLinks" :key="link">
              <a href="#shop" class="font-sans text-sm text-gray-600 hover:text-naturally-green transition-colors">
                {{ link }}
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 3: About Links -->
        <div>
          <h4 class="font-serif font-bold text-naturally-dark text-base mb-4">
            About Us
          </h4>
          <ul class="space-y-2.5">
            <li v-for="link in aboutLinks" :key="link">
              <a href="#about" class="font-sans text-sm text-gray-600 hover:text-naturally-green transition-colors">
                {{ link }}
              </a>
            </li>
          </ul>
        </div>

        <!-- Column 4: Legal & Support -->
        <div>
          <h4 class="font-serif font-bold text-naturally-dark text-base mb-4">
            Legal & Support
          </h4>
          <ul class="space-y-2.5">
            <li v-for="link in legalLinks" :key="link">
              <a href="#footer" class="font-sans text-sm text-gray-600 hover:text-naturally-green transition-colors">
                {{ link }}
              </a>
            </li>
          </ul>
        </div>
      </div>

      <!-- Copyright Bottom Bar -->
      <div id="footer" class="pt-8 border-t border-gray-200 text-center font-sans text-sm text-gray-500">
        <p>© 2026 Naturally Skincare Inc. All rights reserved. Made with nature & love.</p>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { Leaf } from 'lucide-vue-next'

const shopLinks = [
  'Face Care',
  'Body Care',
  'Hair Care',
  'Serums & Oils',
  'Our Bestsellers',
  'Gift Cards'
]

const aboutLinks = [
  'Our Story',
  'Sustainability',
  'Botanical Sourcing',
  'Press & Media',
  'Careers'
]

const legalLinks = [
  'Contact Us',
  'Shipping & Returns',
  'Privacy Policy',
  'Terms of Service',
  'FAQ'
]
</script>
