<script setup>
import lightSpeaker0 from '@/assets/images/Speaker/LightMode/image-speaker-0.png'
import lightSpeaker1 from '@/assets/images/Speaker/LightMode/image-speaker-1.png'
import lightSpeaker2 from '@/assets/images/Speaker/LightMode/image-speaker-2.png'
import lightSpeaker3 from '@/assets/images/Speaker/LightMode/image-speaker-3.png'
import darkSpeaker0 from '@/assets/images/Speaker/DarkMode/image-speaker-0.png'
import darkSpeaker1 from '@/assets/images/Speaker/DarkMode/image-speaker-1.png'
import darkSpeaker2 from '@/assets/images/Speaker/DarkMode/image-speaker-2.png'
import darkSpeaker3 from '@/assets/images/Speaker/DarkMode/image-speaker-3.png'
import { computed, ref } from 'vue'

const model = defineModel()
const previousVolume = ref(0)

const props = defineProps({
  isMaster: {
    type: Boolean,
    default: false,
  },
})

const speakers = [
  { light: lightSpeaker0, dark: darkSpeaker0 },
  { light: lightSpeaker1, dark: darkSpeaker1 },
  { light: lightSpeaker2, dark: darkSpeaker2 },
  { light: lightSpeaker3, dark: darkSpeaker3 },
]

function toggleMute() {
  if (model.value !== 0) {
    previousVolume.value = model.value
    model.value = 0
    return
  } else if (previousVolume.value !== 0) {
    model.value = previousVolume.value
  }
}

const currentSpeaker = computed(() => {
  const val = Number(model.value)

  if (val === 0) return speakers[0]
  if (val <= 33) return speakers[1]
  if (val <= 66) return speakers[2]
  return speakers[3]
})
</script>
<template>
  <div
    :class="{
      'flex flex-row m-10 items-center gap-3 justify-between': isMaster,
      'flex flex-col-reverse items-center gap-2': !isMaster,
    }"
  >
    <p v-if="isMaster" class="font-space-grotesk text-pink-400 text-xl">master</p>
    <input
      type="range"
      min="0"
      max="100"
      v-model="model"
      class="h-2 bg-pink-400 rounded-lg appearance-none cursor-pointer accent-slate-950"
      :class="{ 'w-6/10': isMaster, 'w-full': !isMaster }"
    />
    <div class="flex flex-row gap-1 items-center min-w-14 justify-end">
      <img
        :src="currentSpeaker.light"
        class="dark:hidden w-4 cursor-pointer"
        alt="Speaker"
        @click="toggleMute"
      />
      <img
        :src="currentSpeaker.dark"
        class="hidden dark:block w-4 cursor-pointer"
        alt="Speaker"
        @click="toggleMute"
      />
      <p class="text-lg tabular-nums">{{ model }}%</p>
    </div>
  </div>
</template>
