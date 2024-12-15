<template>
  <q-card flat bordered>
    <q-card-section>
      <h5 class="q-my-none">{{ title }}</h5>
    </q-card-section>
    <q-separator inset />
    <q-card-section>
      <slot />
    </q-card-section>
  </q-card>
</template>

<script setup lang="ts">
defineProps<{
  title: string
}>()
</script>
