<template>
  <div class="css-unit-converter">
      <Header />
      <main class="converter-main">
        <Converter
          :input-value="inputValue"
          :input-unit="inputUnit"
          :output-unit="outputUnit"
          :base-font-size="baseFontSize"
          :show-settings="showSettings"
          :copied="copied"
          :result="result"
          :units="units"
          @toggle-settings="toggleSettings"
          @swap-units="swapUnits"
          @copy-to-clipboard="copyToClipboard"
          @update-input-value="updateInputValue"
          @update-input-unit="updateInputUnit"
          @update-output-unit="updateOutputUnit"
          @update-base-font-size="updateBaseFontSize"
        />
        <Conversions
          :conversions="conversions"
          @copy-conversion="copyConversion"
        />
        <Info />
      </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { Icon } from '@iconify/vue'
import Header from './components/Header.vue'
import Info from './components/Info.vue'
import Conversions from './components/Conversions.vue'
import Converter from './components/Converter.vue'

const inputValue = ref('16')
const inputUnit = ref('px')
const outputUnit = ref('rem')
const baseFontSize = ref(16)
const showSettings = ref(false)
const copied = ref(false)
const windowWidth = ref(window.innerWidth)
const windowHeight = ref(window.innerHeight)

const units = ['px', 'rem', 'em', 'vw', 'vh', 'vmin', 'vmax', '%', 'pt', 'cm', 'mm', 'in']

// Atualizar dimensões da janela
const updateWindowDimensions = () => {
  windowWidth.value = window.innerWidth
  windowHeight.value = window.innerHeight
}

// Converter unidades
const convertUnit = (value, from, to) => {
  if (!value || isNaN(value)) return ''
  
  const num = parseFloat(value)
  
  let pixels
  switch (from) {
    case 'px':
      pixels = num
      break
    case 'rem':
    case 'em':
      pixels = num * baseFontSize.value
      break
    case 'vw':
      pixels = (num * windowWidth.value) / 100
      break
    case 'vh':
      pixels = (num * windowHeight.value) / 100
      break
    case 'vmin':
      pixels = (num * Math.min(windowWidth.value, windowHeight.value)) / 100
      break
    case 'vmax':
      pixels = (num * Math.max(windowWidth.value, windowHeight.value)) / 100
      break
    case '%':
      pixels = (num * baseFontSize.value) / 100
      break
    case 'pt':
      pixels = num * 1.333333
      break
    case 'cm':
      pixels = num * 37.795276
      break
    case 'mm':
      pixels = num * 3.7795276
      break
    case 'in':
      pixels = num * 96
      break
    default:
      pixels = num
  }

  let result
  switch (to) {
    case 'px':
      result = pixels
      break
    case 'rem':
    case 'em':
      result = pixels / baseFontSize.value
      break
    case 'vw':
      result = (pixels * 100) / windowWidth.value
      break
    case 'vh':
      result = (pixels * 100) / windowHeight.value
      break
    case 'vmin':
      result = (pixels * 100) / Math.min(windowWidth.value, windowHeight.value)
      break
    case 'vmax':
      result = (pixels * 100) / Math.max(windowWidth.value, windowHeight.value)
      break
    case '%':
      result = (pixels * 100) / baseFontSize.value
      break
    case 'pt':
      result = pixels / 1.333333
      break
    case 'cm':
      result = pixels / 37.795276
      break
    case 'mm':
      result = pixels / 3.7795276
      break
    case 'in':
      result = pixels / 96
      break
    default:
      result = pixels
  }

  return result.toFixed(4).replace(/\.?0+$/, '')
}

// Resultado da conversão principal
const result = computed(() => {
  return convertUnit(inputValue.value, inputUnit.value, outputUnit.value)
})

// Todas as conversões
const conversions = computed(() => {
  return units
    .filter(unit => unit !== inputUnit.value)
    .map(unit => ({
      unit,
      value: convertUnit(inputValue.value, inputUnit.value, unit)
    }))
})

// Funções
const toggleSettings = () => {
  showSettings.value = !showSettings.value
}

const swapUnits = () => {
  const tempUnit = inputUnit.value
  inputUnit.value = outputUnit.value
  outputUnit.value = tempUnit
  inputValue.value = result.value || '0'
}

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(`${result.value}${outputUnit.value}`)
    copied.value = true
    setTimeout(() => (copied.value = false), 2000)
  } catch (err) {
    console.error('Falha ao copiar: ', err)
  }
}

const copyConversion = async (conversion) => {
  try {
    await navigator.clipboard.writeText(`${conversion.value}${conversion.unit}`)
  } catch (err) {
    console.error('Falha ao copiar: ', err)
  }
}

const updateInputValue = (value) => {
  inputValue.value = value
}

const updateInputUnit = (unit) => {
  inputUnit.value = unit
}

const updateOutputUnit = (unit) => {
  outputUnit.value = unit
}

const updateBaseFontSize = (size) => {
  baseFontSize.value = parseFloat(size) || 16
}

onMounted(() => {
  window.addEventListener('resize', updateWindowDimensions)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateWindowDimensions)
})
</script>

<style scoped>

.css-unit-converter {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100vw;
  min-height: 100vh;
  height: auto;
  padding: 1rem;
  padding-right: 2rem;
  background-color: transparent;
}

.converter-main {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  width: 100%;
  height: auto;
  gap: 1.5rem;
}
</style>