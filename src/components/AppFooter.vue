<script setup>
import github from '@/assets/icons/icon-github.svg'
</script>
<template>
  <footer class="flex items-center justify-center gap-3">
    <p class="text-2xl dark:text-white">© 2026 - Naudet Maël</p>
    <a
      href="https://github.com/NovaNoctua/lofi-mixer"
      target="_blank"
      class="cursor-pointer"
      title="Repo of the project"
      ><img
        :src="github"
        alt="Github Icon"
        class="w-10 dark:brightness-0 dark:invert hover:filter-[brightness(0)_saturate(100%)_invert(49%)_sepia(10%)_saturate(993%)_hue-rotate(191deg)_brightness(89%)_contrast(88%)]"
    /></a>
  </footer>
</template>
