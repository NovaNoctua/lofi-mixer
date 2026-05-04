<script setup>
import github from '@/assets/icons/icon-github.svg'
</script>
<template>
  <footer class="flex items-center justify-center gap-3">
    <p class="text-2xl">© 2026 - Naudet Maël</p>
    <a href="https://github.com/NovaNoctua" target="_blank" class="cursor-pointer"
      ><img :src="github" alt="Github Icon" class="w-10 dark:brightness-0 dark:invert"
    /></a>
  </footer>
</template>
