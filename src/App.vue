<script setup>
// Modules
import { computed, onMounted, onUnmounted, ref, markRaw, watch } from 'vue'
import localforage from 'localforage'

// Components
import MobileHeader from './components/MobileHeader.vue'
import VolumeSlider from './components/VolumeSlider.vue'
import AppFooter from './components/AppFooter.vue'
import MusicCard from './components/MusicCard.vue'
import AddMusicCard from './components/AddMusicCard.vue'
import AddMusicPopup from './components/AddMusicPopup.vue'
import { defaultTracksData } from './data/defaultTrack'
import DesktopHeader from './components/DesktopHeader.vue'
import { initTheme } from './stores/theme'

const masterVolume = ref(50)

const musics = ref(
  defaultTracksData.map((track) => ({
    title: track.title,
    image: track.image,
    audio: markRaw(new Audio(track.audioSrc)),
    isPlaying: false,
    volume: 50,
    isCustom: false,
  })),
)

const activeMusicCount = computed(() => musics.value.filter((m) => m.isPlaying).length)
const isPopupVisible = ref(false)

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

async function receiveNewTrack(payload) {
  try {
    const existingTracks = (await localforage.getItem('custom-tracks')) || []

    const newTrack = {
      id: Date.now(),
      title: payload.title,
      image: payload.image,
      audio: payload.audio,
    }

    existingTracks.push(newTrack)
    await localforage.setItem('custom-tracks', existingTracks)

    musics.value.push({
      id: newTrack.id,
      title: payload.title,
      image: URL.createObjectURL(payload.image),
      audio: markRaw(new Audio(URL.createObjectURL(payload.audio))),
      isPlaying: false,
      volume: 50,
      isCustom: true,
    })
  } catch (e) {
    console.error('Failed to save track:', e)
  }
}

async function removeTrack(index) {
  const trackToDelete = musics.value[index]
  URL.revokeObjectURL(trackToDelete.image)
  URL.revokeObjectURL(trackToDelete.audio.src)

  musics.value.splice(index, 1)

  if (trackToDelete.id) {
    try {
      const storedTracks = (await localforage.getItem('custom-tracks')) || []
      const updatedTracks = storedTracks.filter((track) => track.id !== trackToDelete.id)
      await localforage.setItem('custom-tracks', updatedTracks)
    } catch (e) {
      console.error('Failed to delete track from storage:', e)
    }
  }
}

watch(masterVolume, () => {
  updateAllVolumes()
})

onMounted(async () => {
  initTheme()
  try {
    const storedTracks = (await localforage.getItem('custom-tracks')) || []

    const loadedTracks = storedTracks.map((track) => ({
      id: track.id,
      title: track.title,
      image: URL.createObjectURL(track.image),
      audio: markRaw(new Audio(URL.createObjectURL(track.audio))),
      isPlaying: false,
      volume: 50,
      isCustom: true,
    }))

    musics.value.push(...loadedTracks)
  } catch (e) {
    console.error('Failed to load custom tracks:', e)
  }

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
    <MobileHeader class="md:hidden" />
    <DesktopHeader class="hidden md:flex" v-model="masterVolume" :musics="musics" />
    <main class="flex-1">
      <p class="text-slate-500 text-lg mb-10 dark:text-slate-400 md:text-2xl">
        Absolute silence is terrible for focus, and regular music eventually gets distracting. I
        built this mixer because I just wanted the sound of a coffee shop and a thunderstorm playing
        at the same time. Pick from the default sounds, or upload your own audio files directly into
        the browser. Adjust the volume sliders until it feels right, and leave the tab open in the
        background. Your uploaded files stay on your machine. No accounts, no paywalls, just exactly
        the noise you need.
      </p>

      <VolumeSlider class="md:hidden" is-master v-model="masterVolume" />

      <section>
        <div class="flex justify-between items-center">
          <h2 class="text-4xl font-bold dark:text-white">The Mixer</h2>
          <p class="font-space-grotesk text-xl dark:text-white md:hidden">
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
              :is-custom="music.isCustom"
              v-model="music.volume"
              @update:model-value="() => updateVolume(index)"
              @toggle-audio-state="toggleAudioState(index)"
              @delete-music="removeTrack(index)"
            />
            <AddMusicCard @click="isPopupVisible = true" />
          </div>
        </div>
      </section>
      <section>
        <AddMusicPopup
          v-if="isPopupVisible"
          @close="isPopupVisible = false"
          @add-custom-track="receiveNewTrack"
        />
      </section>
    </main>
    <AppFooter />
  </div>
</template>
