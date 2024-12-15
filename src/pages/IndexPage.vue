<template>
  <q-page class="app-wrapper row justify-evenly items-center q-py-xl">
    <div class="app-container bg-white q-py-xl q-px-lg rounded-borders custom-shadow-light">
      <h2 class="q-mt-none text-center q-mb-xs">Color Generator App</h2>

      <CustomCard title="Starts/Stop generation of three generated numbers">
        <div class="q-gutter-x-md">
          <q-btn class="app-custom-btn" label="Start" icon="play_arrow"></q-btn>
          <q-btn class="app-custom-btn" label="Stop" icon="stop"></q-btn>
        </div>
      </CustomCard>

      <CustomCard title="The decimal values of the three generated numbers">
        <code class="bg-dark text-white q-pa-xs app-rgba-color">92 247 219</code>
      </CustomCard>

      <CustomCard title="The CSS hex RGB color representation of the three numbers">
        <code class="q-pa-xs app-hex-color text-weight-bold" style="color: #5cf7db">#5cf7db</code>
      </CustomCard>

      <CustomCard
        title="The numbers of colors generated so far. The reset button resets the counter"
      >
        <p class="q-pa-xs app-hex-color">Updates Count: <span>68</span></p>
        <q-btn class="app-custom-btn" label="Reset" icon="restart_alt"></q-btn>
      </CustomCard>

      <CustomCard title="Same as above but counting has a three seconds delay">
        <p class="q-pa-xs app-hex-color">Updates Count: <span>68</span></p>
        <q-btn class="app-custom-btn" label="Reset" icon="restart_alt"></q-btn>
      </CustomCard>

      <CustomCard
        title="Dynamically select one of the three views above (decimal value, colored hex or counter).
      If the checkbox is checked applies a three seconds delay to the view."
      >
        <div class="row q-gutter-md q-mb-lg">
          <div class="col">
            <q-select v-model="selectModel" :options="selectOptions" label="Standard"></q-select>
          </div>
          <div class="col">
            <q-checkbox class="q-mt-md" v-model="checkboxModel" label="Delayed" />
          </div>
        </div>
        <div>
          <h2>Result</h2>
        </div>
      </CustomCard>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import CustomCard from 'components/CustomCard.vue'

const selectModel = defineModel()
const checkboxModel = defineModel('checkboxModel', { default: false })

const selectOptions = [
  { label: 'Decimal value', value: 'decimal' },
  { label: 'Hex Colored', value: 'hex' },
  { label: 'Counter', value: 'counter' },
]
</script>
