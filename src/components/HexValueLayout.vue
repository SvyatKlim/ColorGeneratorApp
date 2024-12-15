<template>
  <code class="q-pa-xs app-hex-color text-weight-bold" :style="{ color: value }">{{ value }}</code>
</template>

<script setup lang="ts">

import { computed } from 'vue'

const props = defineProps<{
  decimalValue: number[]
}>()

const value = computed(() => {
  return RGBToHex()
})

const toHex = (value:number) => {
  const hex = value.toString(16)
  return hex.length === 1 ? '0' + hex : hex
}

const RGBToHex = () => {
  const [r, g, b] = props.decimalValue
  return`#${toHex(r)}${toHex(g)}${toHex(b)}`
}

</script>
