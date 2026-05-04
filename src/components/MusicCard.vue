<script setup>
import { ref } from 'vue'
import VolumeSlider from './VolumeSlider.vue'
import pause from '@/assets/icons/icon-pause.svg'
import resume from '@/assets/icons/icon-resume.svg'

const props = defineProps(['image', 'title', 'isPlaying'])
const model = defineModel()

const emits = defineEmits(['toggle-audio-state'])

function toggleAudioState() {
  emits('toggle-audio-state')
}
</script>
<template>
  <div
    class="relative mt-16 flex flex-col items-center rounded-3xl border-2 border-slate-300 py-6 px-4 pt-0 shadow-xl"
  >
    <img
      :src="image"
      alt="Audio Image"
      class="-mt-20 mb-6 w-full rounded-2xl object-cover shadow-lg"
    />

    <p class="mb-3 text-xl font-medium">{{ title }}</p>

    <button @click="toggleAudioState" class="mb-3 cursor-pointer">
      <img :src="isPlaying ? pause : resume" alt="Play/Pause" class="w-10" />
    </button>

    <VolumeSlider v-model="model" />
  </div>
</template>
