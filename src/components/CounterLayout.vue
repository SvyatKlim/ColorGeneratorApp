<template>
  <p class="q-pa-xs app-hex-color">
    Updates Count: <span>{{ count }}</span>
  </p>
  <q-btn
    @click="count = 0"
    class="app-custom-btn"
    label="Reset"
    icon="restart_alt"
  ></q-btn></template>

<script setup lang="ts">
const count = defineModel()
</script>
