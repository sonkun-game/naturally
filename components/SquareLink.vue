<template>
  <NuxtLink
    :to="link"
    class="group relative block aspect-square overflow-hidden rounded-2xl border border-white/10 bg-slate-900 shadow-lg transition duration-300 hover:-translate-y-1 hover:border-cyan-400/60 hover:shadow-cyan-950/50 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-cyan-400"
  >
    <img
      :src="image"
      :alt="title"
      class="absolute inset-0 h-full w-full object-cover transition duration-500 group-hover:scale-105"
      loading="lazy"
    />
    <div class="absolute inset-0 bg-gradient-to-t from-slate-950/90 via-slate-950/10 to-transparent" />
    <h2 class="absolute inset-x-5 bottom-5 text-xl font-semibold text-white drop-shadow sm:text-2xl">
      {{ title }}
    </h2>
  </NuxtLink>
</template>

<script setup lang="ts">
defineProps<{
  image: string
  title: string
  link: string
}>()
</script>
