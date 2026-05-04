<script setup>
import add from '@/assets/icons/icon-add.svg'
</script>
<template>
  <div
    class="mt-16 flex flex-col justify-center items-center rounded-3xl border-2 border-slate-300 py-6 px-4 pt-0 shadow-xl gap-5 cursor-pointer hover:bg-pink-100"
  >
    <img
      :src="add"
      alt="Add button"
      class="w-17"
      style="
        filter: brightness(0) saturate(100%) invert(71%) sepia(33%) saturate(7440%)
          hue-rotate(295deg) brightness(102%) contrast(97%);
      "
    />
    <p class="font-semibold text-3xl text-pink-400">Add a music</p>
  </div>
</template>
