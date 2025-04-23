<script setup lang="ts">
import { VitePwaManifest } from "#components"
</script>

<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtPage />
    <PWAInstallButton />
    <VitePwaManifest />
  </div>
</template>
