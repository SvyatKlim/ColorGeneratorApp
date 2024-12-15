<template>
  <code class="bg-dark text-white q-pa-xs app-rgba-color">{{ value }}</code>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  decimalValue: number[]
}>()

const value = computed(() => {
  return props.decimalValue.join(' ')
})
</script>
