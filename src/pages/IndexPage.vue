<template>
  <q-page class="app-wrapper row justify-evenly items-center q-py-xl">
    <div class="app-container bg-white q-py-xl q-px-lg rounded-borders custom-shadow-light">
      <h2 class="q-mt-none text-center q-mb-xs">Color Generator App</h2>

      <CustomCard title="Start/Stop generation of three generated numbers">
        <div class="q-gutter-x-md">
          <q-btn
            @click="startGeneration"
            class="app-custom-btn"
            label="Start"
            icon="play_arrow"
            :disable="state.inProgress"
          ></q-btn>
          <q-btn @click="stopGeneration" class="app-custom-btn" label="Stop" icon="stop"></q-btn>
        </div>
      </CustomCard>

      <CustomCard title="The decimal values of the three generated numbers">
        <DecimalValueLayout :decimal-value="state.decimal" />
      </CustomCard>

      <CustomCard title="The CSS hex RGB color representation of the three numbers">
        <HexValueLayout :decimal-value="state.decimal" />
      </CustomCard>

      <CustomCard
        title="The numbers of colors generated so far. The reset button resets the counter"
      >
        <CounterLayout v-model="state.count" />
      </CustomCard>

      <CustomCard title="Same as above but counting has a three seconds delay">
        <CounterLayout v-model="state.delayedCount" />
      </CustomCard>

      <CustomCard
        title="Dynamically select one of the three views above (decimal value, colored hex or counter).
      If the checkbox is checked applies a three seconds delay to the view."
      >
        <div class="row q-gutter-md q-mb-lg">
          <div class="col">
            <q-select
              v-model="selectModel"
              :options="selectOptions"
              label="Standard"
              emit-value
              map-options
              @update:model-value="renderSelectedOptions"
            ></q-select>
          </div>
          <div class="col">
            <q-checkbox
              class="q-mt-md"
              v-model="state.isDelayed"
              label="Delayed"
              @update:model-value="renderSelectedOptions"
            />
          </div>
        </div>
        <div v-if="selectModel === 'decimal'">
          <DecimalValueLayout :decimal-value="optionalModel as number[]" />
        </div>
        <div v-if="selectModel === 'hex'">
          <HexValueLayout :decimal-value="optionalModel as number[]" />
        </div>

        <div v-if="selectModel === 'counter'">
          <CounterLayout v-model="optionalModel" />
        </div>
      </CustomCard>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import CustomCard from 'components/CustomCard.vue'
import { reactive, ref, watch, type Ref } from 'vue'
import DecimalValueLayout from 'components/DecimalValueLayout.vue'
import HexValueLayout from 'components/HexValueLayout.vue'
import CounterLayout from 'components/CounterLayout.vue'

interface ComponentState {
  decimal: number[]
  count: number
  delayedCount: number
  dropdownDelayedCount: number
  inProgress: boolean
  isDelayed: boolean
  colorsHistory: number[][]
}

const state = reactive<ComponentState>({
  decimal: [0, 0, 0],
  count: 0,
  dropdownDelayedCount: 0,
  delayedCount: 0,
  inProgress: false,
  isDelayed: false,
  colorsHistory: [],
})

const selectModel: Ref<'decimal' | 'hex' | 'counter'> = ref('counter')
const selectOptions = [
  { label: 'Decimal value', value: 'decimal' },
  { label: 'Hex Colored', value: 'hex' },
  { label: 'Counter', value: 'counter' },
]

const optionalModel: Ref<number | number[]> = ref(0)

let interval: NodeJS.Timeout | null = null
let delayedInterval: NodeJS.Timeout | null = null

const getRandomNumber = (min = 0, max = 255): number =>
  Math.trunc(Math.random() * (max - min) + min)

const randomizeDecimal = (): void => {
  state.decimal = Array.from({ length: 3 }, () => getRandomNumber())
  state.colorsHistory.push([...state.decimal])
  if (state.colorsHistory.length > 4) state.colorsHistory.shift()
}

const getDefaultValue = (): number | number[] => {
  if (state.isDelayed) {
    switch (selectModel.value) {
      case 'decimal':
      case 'hex':
        return state.colorsHistory.length > 3 && state.colorsHistory[0]
          ? state.colorsHistory[0]
          : [0, 0, 0]
      default:
        return state.colorsHistory.length > 3 ? state.delayedCount : 0
    }
  } else {
    switch (selectModel.value) {
      case 'decimal':
      case 'hex':
        return state.decimal
      default:
        return state.count
    }
  }
}
const toggleSelectedValue = (): number | number[] => {
  if (state.isDelayed) {
    switch (selectModel.value) {
      case 'decimal':
      case 'hex':
        return state.colorsHistory.length > 3 && state.colorsHistory[0]
          ? state.colorsHistory[0]
          : [0, 0, 0]
      default:
        return state.colorsHistory.length > 3 && typeof optionalModel.value === 'number' ? optionalModel.value + 1 : 0
    }
  } else {
    switch (selectModel.value) {
      case 'decimal':
      case 'hex':
        return state.decimal
      default:
        return (optionalModel.value as number) + 1
    }
  }
}

const renderSelectedOptions = (): void => {
  optionalModel.value = getDefaultValue()
}

const startGeneration = (): void => {
  state.inProgress = true
  interval = setInterval(() => {
    randomizeDecimal()
    state.count++
  }, 1000)
  setTimeout(() => {
    delayedInterval = setInterval(() => {
      state.delayedCount++
    }, 1000)
  }, 3000)
}

const stopGeneration = (): void => {
  if (interval) clearInterval(interval)
  setTimeout(() => {
    if (delayedInterval) clearInterval(delayedInterval)
    state.inProgress = false
  }, 3000)
}

watch(
  () => state.decimal,
  () => {
    optionalModel.value = toggleSelectedValue()
  },
)
</script>
