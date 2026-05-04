<script setup>
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

    <p class="mb-3 text-xl w-full font-medium truncate text-center">{{ title }}</p>

    <button
      @click="toggleAudioState"
      class="mb-3 cursor-pointer hover:filter-[brightness(0)_saturate(100%)_invert(97%)_sepia(54%)_saturate(6067%)_hue-rotate(279deg)_brightness(93%)_contrast(111%)_!important]"
      style="
        filter: brightness(0) saturate(100%) invert(71%) sepia(33%) saturate(7440%)
          hue-rotate(295deg) brightness(102%) contrast(97%);
      "
    >
      <img :src="isPlaying ? pause : resume" alt="Play/Pause" class="w-10" />
    </button>

    <VolumeSlider v-model="model" />
  </div>
</template>
