<script setup>
import { onMounted, ref } from 'vue'

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
      class="w-14 h-7 flex items-center bg-gray-300 rounded-full p-1 duration-300 ease-in-out border"
      :class="isDark ? 'border-white bg-neutral-800' : 'bg-neutral-100 border-black'"
    >
      <div
        class="w-5 h-5 rounded-full shadow-md transform duration-300 ease-in-out"
        :class="isDark ? 'bg-neutral-100' : 'translate-x-7 bg-black'"
      ></div>
    </div>
  </div>
</template>
