<template>
  <main class="min-h-screen bg-slate-950 px-6 py-12 text-white">
    <section class="mx-auto flex min-h-[calc(100vh-6rem)] w-full max-w-6xl flex-col justify-center">
      <header class="mb-10 text-center">
        <h1 class="text-3xl font-bold tracking-tight sm:text-4xl">Panorama Worlds</h1>
        <p class="mt-3 text-sm text-slate-400 sm:text-base">Chọn một thế giới để khám phá</p>
      </header>

      <div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
        <SquareLink
          title="Cyber Punk World"
          link="/paranoma/cyberpunk"
          image="/image/paranoma.png"
        />
        <SquareLink
          title="Ocean World"
          link="/paranoma/ocean"
          image="/img/paranoma/ocean/ocean.png"
        />
        <SquareLink
          title="Cyber Punk World"
          link="/paranoma/cyberpunk"
          image="/image/paranoma.png"
        />
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
import SquareLink from '~/components/SquareLink.vue'
</script>
