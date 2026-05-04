<script setup>
// Vue
import { computed, onMounted, onUnmounted, ref, markRaw } from 'vue'

// Assets
import rainAudio from '@/assets/audio/audio-rain.mp3'
import rainImage from '@/assets/images/musics/image-rain.png'
import brownImage from '@/assets/images/musics/image-brown.png'
import brownAudio from '@/assets/audio/audio-brown.mp3'
import vinylImage from '@/assets/images/musics/image-vinyl.png'
import vinylAudio from '@/assets/audio/audio-vinyl.mp3'

// Components
import MobileHeader from './components/MobileHeader.vue'
import VolumeSlider from './components/VolumeSlider.vue'
import AppFooter from './components/AppFooter.vue'
import MusicCard from './components/MusicCard.vue'
import AddMusicCard from './components/AddMusicCard.vue'

const masterVolume = ref(50)

const musics = ref([
  {
    title: 'Rain Noise',
    image: rainImage,
    audio: markRaw(new Audio(rainAudio)),
    isPlaying: false,
    volume: 50,
  },
  {
    title: 'Brown Noise',
    image: brownImage,
    audio: markRaw(new Audio(brownAudio)),
    isPlaying: false,
    volume: 50,
  },
  {
    title: 'Vinyl Crack',
    image: vinylImage,
    audio: markRaw(new Audio(vinylAudio)),
    isPlaying: false,
    volume: 50,
  },
])

const activeMusicCount = computed(() => musics.value.filter((m) => m.isPlaying).length)

function toggleAudioState(index) {
  musics.value[index].isPlaying = !musics.value[index].isPlaying

  if (musics.value[index].isPlaying) {
    musics.value[index].audio.play()
  } else {
    musics.value[index].audio.pause()
  }
}

function updateAllVolumes() {
  for (const music of musics.value) {
    music.audio.volume = (music.volume / 100) * (masterVolume.value / 100)
  }
}

function updateVolume(index) {
  musics.value[index].audio.volume = (musics.value[index].volume / 100) * (masterVolume.value / 100)
}

onMounted(() => {
  for (const music of musics.value) {
    music.audio.loop = true
    music.audio.volume = (music.volume / 100) * (masterVolume.value / 100)
  }
})

onUnmounted(() => {
  for (const music of musics.value) {
    music.audio.pause()
  }
})
</script>

<template>
  <div class="flex flex-col px-10 py-5 font-quicksand bg-gray-100 dark:bg-slate-950 min-h-screen">
    <MobileHeader />
    <main class="flex-1">
      <p class="text-slate-500 text-lg mb-10">
        Absolute silence is terrible for focus, and regular music eventually gets distracting. I
        built this mixer because I just wanted the sound of a coffee shop and a thunderstorm playing
        at the same time. Pick from the default sounds, or upload your own audio files directly into
        the browser. Adjust the volume sliders until it feels right, and leave the tab open in the
        background. Your uploaded files stay on your machine. No accounts, no paywalls, just exactly
        the noise you need.
      </p>
      <VolumeSlider is-master v-model="masterVolume" @update:model-value="updateAllVolumes" />
      <section>
        <div class="flex justify-between items-center">
          <h2 class="text-4xl font-bold">The Mixer</h2>
          <p class="font-space-grotesk text-xl">
            <span class="text-pink-400">{{ activeMusicCount }}/</span> {{ musics.length }}
            <span class="text-pink-400">active</span>
          </p>
        </div>
        <div class="w-full py-20">
          <div
            class="grid grid-cols-1 auto-rows-fr sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-x-5 gap-y-24 w-full"
          >
            <MusicCard
              v-for="(music, index) in musics"
              :image="music.image"
              :title="music.title"
              :isPlaying="music.isPlaying"
              v-model="music.volume"
              @update:model-value="() => updateVolume(index)"
              @toggle-audio-state="toggleAudioState(index)"
            />
            <AddMusicCard />
          </div>
        </div>
      </section>
    </main>
    <AppFooter />
  </div>
</template>
