<script setup>
import { computed, ref } from 'vue'
import noImage from '@/assets/images/image-nothing.png'

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
    <div class="bg-gray-100 border-6 border-pink-400 cursor-default p-5 rounded-2xl" @click.stop>
      <h2>Add you own music</h2>
      <form @submit.prevent="handleSubmit">
        <div class="flex flex-col">
          <label for="title">Title</label>
          <input type="text" v-model="title" id="title" placeholder="e. g. Fire Crack" />
        </div>
        <div>
          <label for="audio">Import audio</label>
          <div>
            <span>
              {{ audioName }}
            </span>
            <button type="button" @click="triggerAudioInput">Browse files</button>
            <input
              type="file"
              ref="hiddenAudioFile"
              @change="handleAudioChange"
              class="hidden"
              accept="audio/mp3, audio/wav"
            />
          </div>
        </div>
        <div>
          <label for="image">Import image</label>
          <div>
            <button @click="triggerImageInput" type="button">
              <img :src="previewImage" alt="Image Preview" />
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
        <p v-if="error" class="text-red-600">{{ error }}</p>
        <button class="cursor-pointer" type="submit">Add music</button>
      </form>
    </div>
  </div>
</template>
