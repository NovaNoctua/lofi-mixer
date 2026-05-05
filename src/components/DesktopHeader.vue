<script setup>
import { computed } from 'vue'
import DarkToggle from './DarkToggle.vue'
import VolumeSlider from './VolumeSlider.vue'

const props = defineProps(['musics'])
const volumeModel = defineModel()

const activeMusicCount = computed(() => props.musics.filter((m) => m.isPlaying).length)
</script>
<template>
  <header class="flex justify-between mt-5 mb-10 items-center">
    <h1 class="font-space-grotesk text-5xl font-bold dark:text-white lg:text-8xl">Lofi Mixer</h1>
    <p class="font-space-grotesk text-2xl dark:text-white">
      <span class="text-pink-400">{{ activeMusicCount }}/</span> {{ musics.length }}
      <span class="text-pink-400">active</span>
    </p>
    <VolumeSlider is-master v-model="volumeModel" />
    <DarkToggle />
  </header>
</template>
