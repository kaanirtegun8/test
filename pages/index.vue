<template>
  <div style="display: flex; justify-content: center; align-items: center; height: 100vh;">
    <button 
      v-if="showInstallButton"
      @click="handleInstallClick"
      style="padding: 15px 30px; background: blue; color: white; border: none; border-radius: 5px; font-size: 16px;"
    >
      Uygulamayı Yükle
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

const showInstallButton = ref(false)
let deferredPrompt: any = null

const handleInstallClick = async () => {
  if (!deferredPrompt) return
  deferredPrompt.prompt()
  const { outcome } = await deferredPrompt.userChoice
  deferredPrompt = null
  showInstallButton.value = false
}

onMounted(() => {
  window.addEventListener('beforeinstallprompt', (e) => {
    e.preventDefault()
    deferredPrompt = e
    showInstallButton.value = true
  })
})
</script> 