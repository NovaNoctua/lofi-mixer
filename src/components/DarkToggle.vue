<script setup>
import moon from '@/assets/images/image-moon.png'
import sun from '@/assets/images/image-sun.png'

import { isDark, toggleTheme } from '@/stores/theme'
</script>

<template>
  <div class="flex justify-between items-center cursor-pointer gap-1" @click="toggleTheme">
    <div
      class="w-14 h-7 flex items-center justify-between bg-gray-300 rounded-full p-1 duration-300 ease-in-out border md:w-20 md:h-10"
      :class="isDark ? 'border-white bg-neutral-800' : 'bg-neutral-100 border-black'"
    >
      <div
        class="w-5 h-5 rounded-full shadow-md transform duration-300 ease-in-out md:w-7 md:h-7"
        :class="isDark ? 'bg-neutral-100' : 'translate-x-7 bg-black md:translate-x-11'"
      ></div>
      <img
        class="h-4 w-4 mr-1 md:h-6 md:w-6"
        :class="{ '-translate-x-6.5 md:-translate-x-10.5': !isDark }"
        :src="isDark ? moon : sun"
        alt="Dark Mode Icon"
      />
    </div>
  </div>
</template>
