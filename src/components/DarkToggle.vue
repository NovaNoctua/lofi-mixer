<script setup>
import { onMounted, ref } from 'vue'

import moon from '@/assets/images/image-moon.png'
import sun from '@/assets/images/image-sun.png'

const isDark = ref(false)

const toggleTheme = () => {
  isDark.value = !isDark.value

  if (isDark.value) {
    document.documentElement.classList.add('dark')
    localStorage.setItem('theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark')
    localStorage.setItem('theme', 'light')
  }
}

onMounted(() => {
  const savedTheme = localStorage.getItem('theme')

  if (savedTheme === 'dark' || !savedTheme) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
})
</script>

<template>
  <div class="flex justify-between items-center cursor-pointer gap-1" @click="toggleTheme">
    <div
      class="w-14 h-7 flex items-center justify-between bg-gray-300 rounded-full p-1 duration-300 ease-in-out border"
      :class="isDark ? 'border-white bg-neutral-800' : 'bg-neutral-100 border-black'"
    >
      <div
        class="w-5 h-5 rounded-full shadow-md transform duration-300 ease-in-out"
        :class="isDark ? 'bg-neutral-100' : 'translate-x-7 bg-black'"
      ></div>
      <img
        class="h-4 w-4 mr-1"
        :class="{ '-translate-x-6.5': !isDark }"
        :src="isDark ? moon : sun"
        alt="Dark Mode Icon"
      />
    </div>
  </div>
</template>
