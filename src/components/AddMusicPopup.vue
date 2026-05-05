<script setup>
import { computed, ref } from 'vue'
import noImage from '@/assets/images/image-nothing.png'
import close from '@/assets/icons/icon-close.svg'

const emits = defineEmits(['close', 'add-custom-track'])

const title = ref('')
const audioFile = ref(null)
const imageFile = ref(null)

const audioName = ref('no file')
const hiddenAudioFile = ref(null)

const triggerAudioInput = () => {
  hiddenAudioFile.value.click()
}

const handleAudioChange = (event) => {
  const file = event.target.files[0]

  if (file) {
    audioName.value = file.name
    audioFile.value = file
  } else {
    audioName.value = 'no file'
  }
}

const previewImage = ref(noImage)
const hiddenImageFile = ref(null)

const triggerImageInput = () => {
  hiddenImageFile.value.click()
}

const handleImageChange = (event) => {
  const file = event.target.files[0]

  if (file) {
    previewImage.value = file
    imageFile.value = file
    previewImage.value = URL.createObjectURL(file)
  }
}

const error = ref('')

const isReady = computed(() => {
  return title.value !== '' && audioFile.value && imageFile.value
})

async function handleSubmit() {
  if (isReady.value) {
    emits('add-custom-track', {
      title: title.value,
      image: imageFile.value,
      audio: audioFile.value,
    })

    title.value = ''
    imageFile.value = null
    audioFile.value = null

    emits('close')
  } else {
    error.value = 'You must fill all fields before continuing'
  }
}
</script>
<template>
  <div
    class="fixed inset-0 bg-[rgba(0,0,0,0.7)] flex items-center justify-center z-100 cursor-pointer"
    @click="emits('close')"
  >
    <div
      class="bg-gray-100 border-6 border-pink-400 cursor-default px-15 py-10 rounded-2xl w-9/10 dark:bg-slate-950 dark:text-white"
      @click.stop
    >
      <div class="flex flex-row justify-between items-center mb-10">
        <h2 class="text-2xl font-bold">Add your own music</h2>
        <button class="cursor-pointer w-10" @click="emits('close')">
          <img
            :src="close"
            alt="ClosePopup"
            class="hover:filter-[brightness(0)_saturate(100%)_invert(49%)_sepia(10%)_saturate(993%)_hue-rotate(191deg)_brightness(89%)_contrast(88%)] dark:filter-[brightness(100)_saturate(100%)]"
          />
        </button>
      </div>

      <form @submit.prevent="handleSubmit" class="flex flex-col gap-5">
        <div class="flex flex-col gap-3">
          <label for="title" class="text-slate-500 text-xl font-semibold dark:text-slate-400"
            >Title</label
          >
          <input
            class="w-full rounded-lg p-3 border border-slate-600 focus:outline-0 focus:border-pink-400"
            type="text"
            v-model="title"
            id="title"
            placeholder="e. g. Fire Crack"
          />
        </div>
        <div class="flex flex-col gap-3">
          <label for="audio" class="text-slate-500 text-xl font-semibold dark:text-slate-400"
            >Import audio</label
          >
          <div class="flex flex-row justify-between items-center">
            <span class="font-bold text-xl truncate">
              {{ audioName }}
            </span>
            <button
              type="button"
              @click="triggerAudioInput"
              class="border border-slate-600 rounded-full py-2 px-4 hover:border-pink-400 dark:hover:text-pink-400 cursor-pointer"
            >
              Browse files
            </button>
            <input
              type="file"
              ref="hiddenAudioFile"
              @change="handleAudioChange"
              class="hidden"
              accept="audio/mp3, audio/wav"
            />
          </div>
        </div>
        <div class="flex flex-col gap-3">
          <label for="image" class="text-slate-500 text-lg dark:text-slate-400">Import image</label>
          <div>
            <button
              @click="triggerImageInput"
              type="button"
              class="border-2 rounded-2xl border-pink-400 w-full cursor-pointer hover:brightness-90 overflow-hidden"
            >
              <img :src="previewImage" alt="Image Preview" class="object-cover w-full h-[30vh]" />
            </button>
            <input
              type="file"
              ref="hiddenImageFile"
              @change="handleImageChange"
              class="hidden"
              accept="image/*"
            />
          </div>
        </div>
        <p class="text-red-600">{{ error }}</p>
        <button
          class="cursor-pointer bg-pink-400 text-gray-100 hover:bg-pink-300 w-[60%] m-auto text-xl font-bold py-4 rounded-full"
          type="submit"
        >
          Add music
        </button>
      </form>
    </div>
  </div>
</template>
