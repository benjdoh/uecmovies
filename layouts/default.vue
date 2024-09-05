<script setup lang="ts">
import '@unocss/reset/tailwind.css'
</script>

<template>
  <div class="sticky top-0 border-b border-[#f00] bg-black">
    <div class="max-w-5xl mx-auto w-full flex">
      <div class="flex-grow h-20 flex items-center">
        <nuxt-link class="h-20 p-2">
          <svg-logo />
        </nuxt-link>
      </div>

      <div class="flex flex-col">
        <div></div>

        <div></div>
      </div>
    </div>
  </div>

  <slot />

  <div class="sticky bottom-0">what</div>
</template>

<style>
body,
html {
  @apply bg-black text-white;
}
</style>
