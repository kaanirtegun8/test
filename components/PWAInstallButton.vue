<template>
  <div class="fixed bottom-4 right-4">
    <button
      v-if="showInstallButton"
      @click="handleInstallClick"
      class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg shadow-lg transition-colors"
    >
      Install App
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

const showInstallButton = ref(false)
let deferredPrompt: any = null

const handleInstallClick = async () => {
  if (!deferredPrompt) return

  // Show the install prompt
  deferredPrompt.prompt()

  // Wait for the user to respond to the prompt
  const { outcome } = await deferredPrompt.userChoice

  if (outcome === 'accepted') {
    console.log('User accepted the install prompt')
  } else {
    console.log('User dismissed the install prompt')
  }

  // Clear the deferred prompt
  deferredPrompt = null
  showInstallButton.value = false
}

onMounted(() => {
  window.addEventListener('beforeinstallprompt', (e) => {
    // Prevent Chrome 67 and earlier from automatically showing the prompt
    e.preventDefault()
    // Stash the event so it can be triggered later
    deferredPrompt = e
    // Show the install button
    showInstallButton.value = true
  })

  window.addEventListener('appinstalled', () => {
    // Hide the install button
    showInstallButton.value = false
    // Clear the deferred prompt
    deferredPrompt = null
    console.log('PWA was installed')
  })
})
</script> 