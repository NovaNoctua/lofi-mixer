<script setup>
import VolumeSlider from './VolumeSlider.vue'
import pause from '@/assets/icons/icon-pause.svg'
import resume from '@/assets/icons/icon-resume.svg'
import trash from '@/assets/icons/icon-trash.svg'

const props = defineProps(['image', 'title', 'isPlaying', 'isCustom'])
const model = defineModel()

const emits = defineEmits(['toggle-audio-state', 'delete-music'])

function toggleAudioState() {
  emits('toggle-audio-state')
}
</script>
<template>
  <div
    class="relative mt-16 flex flex-col items-center rounded-3xl border-2 border-slate-300 py-6 px-4 pt-0 shadow-xl dark:bg-slate-900 dark:border-black"
  >
    <img
      :src="image"
      alt="Audio Image"
      class="-mt-20 mb-6 w-full rounded-2xl h-full object-cover shadow-lg"
    />

    <div class="relative flex items-center justify-between w-full mb-3">
      <p class="text-2xl font-medium truncate text-center w-full px-8 dark:text-white">
        {{ title }}
      </p>
      <button
        v-if="isCustom"
        class="absolute right-0 cursor-pointer"
        title="Delete Music"
        @click="emits('delete-music')"
      >
        <img
          :src="trash"
          alt="delete"
          class="w-4 hover:filter-[brightness(0)_saturate(100%)_invert(49%)_sepia(10%)_saturate(993%)_hue-rotate(191deg)_brightness(89%)_contrast(88%)]"
        />
      </button>
    </div>

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
