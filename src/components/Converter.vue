<template>
  <section class="converter-section">
    <div class="converter-section__header">
      <h2 class="converter-section__title">Converter</h2>
      <button 
        class="converter-section__settings-button"
        @click="emit('toggleSettings')"
        :class="{ 'converter-section__settings-button--active': showSettings }"
        aria-label="Configurações"
      >
        <Icon icon="material-symbols:settings" class="converter-section__settings-icon" />
      </button>
    </div>

    <transition name="slide-fade">
      <div v-if="showSettings" class="converter-settings">
        <label class="converter-settings__label">
          Tamanho base da fonte (para rem/em)
        </label>
        <input
          type="number"
          :value="baseFontSize"
          @input="emit('updateBaseFontSize', $event.target.value)"
          min="1"
          class="converter-settings__input"
        />
      </div>
    </transition>

    <div class="converter-grid">
      <div class="converter-input-group">
        <label class="converter-input-group__label">De</label>
        <input
          type="number"
          :value="inputValue"
          @input="emit('updateInputValue', $event.target.value)"
          placeholder="Digite o valor"
          class="converter-input-group__value-input"
        />
        <select
          :value="inputUnit"
          @change="emit('updateInputUnit', $event.target.value)"
          class="converter-input-group__unit-select"
        >
          <option 
            v-for="unit in units" 
            :key="`input-${unit}`" 
            :value="unit"
          >
            {{ unit }}
          </option>
        </select>
      </div>

      <div class="converter-swap">
        <button
          @click="emit('swapUnits')"
          class="converter-swap__button"
          title="Inverter unidades"
          aria-label="Inverter unidades"
        >
          <Icon icon="mdi:swap-horizontal" class="converter-swap__icon" />
        </button>
      </div>

      <div class="converter-output-group">
        <label class="converter-output-group__label">Para</label>
        <div class="converter-output-group__result">
          <input
            type="text"
            :value="result ? `${result}${outputUnit}` : ''"
            readonly
            class="converter-output-group__result-input"
            placeholder="Resultado"
          />
          <button
            @click="emit('copyToClipboard')"
            class="converter-output-group__copy-button"
            :class="{ 'converter-output-group__copy-button--copied': copied }"
            title="Copiar resultado"
            aria-label="Copiar resultado"
          >
            <Icon 
              :icon="copied ? 'mdi:check' : 'mdi:content-copy'" 
              class="converter-output-group__copy-icon" 
            />
          </button>
        </div>
        <select
          :value="outputUnit"
          @change="emit('updateOutputUnit', $event.target.value)"
          class="converter-output-group__unit-select"
        >
          <option 
            v-for="unit in units" 
            :key="`output-${unit}`" 
            :value="unit"
          >
            {{ unit }}
          </option>
        </select>
      </div>
    </div>
  </section>
</template>

<script setup>
import { Icon } from '@iconify/vue'

const props = defineProps({
  inputValue: {
    type: String,
    required: true
  },
  inputUnit: {
    type: String,
    required: true
  },
  outputUnit: {
    type: String,
    required: true
  },
  baseFontSize: {
    type: Number,
    required: true
  },
  showSettings: {
    type: Boolean,
    required: true
  },
  copied: {
    type: Boolean,
    required: true
  },
  result: {
    type: String,
    required: true
  },
  units: {
    type: Array,
    required: true
  }
})

const emit = defineEmits([
  'toggleSettings',
  'swapUnits',
  'copyToClipboard',
  'updateInputValue',
  'updateInputUnit',
  'updateOutputUnit',
  'updateBaseFontSize'
])
</script>

<style scoped lang="scss">
.converter-section {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  flex-direction: column;
  background: #ffffff8b;
  border-radius: 1rem;
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  width: 100%;
  min-height: 50vh;
  height: auto;

    &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    height: 15%;
    margin-bottom: 0.5rem;

    .converter-section__title {
    font-size: 1.125rem;
    font-weight: 600;
    color: #374151;
    }
    .converter-section__settings-button {
    padding: 0.5rem;
    border-radius: 0.5rem;
    background: transparent;
    border: none;
    cursor: pointer;
    transition: all 0.2s ease;
    &:hover {
    background: #f3f4f6;
    }

    &:focus {
    outline: 2px solid #6366f1;
    outline-offset: 2px;
    }

    .converter-section__settings-button--active {
    background: #f3f4f6;
    }

    .converter-section__settings-icon {
    width: 1.25rem;
    height: 1.25rem;
    color: #4b5563;
    }
    }
  }

    .slide-fade-enter-active,
    .slide-fade-leave-active {
    transition: all 0.3s ease;
    }

    .slide-fade-enter-from,
    .slide-fade-leave-to {
    opacity: 0;
    transform: translateY(-10px);
    }

    .converter-settings {
        display: flex;
        align-items: start;
        justify-content: space-between;
        flex-direction: column;
        width: 100%;
        height: 35%;
        background: #f9fafb;
        border-radius: 0.75rem;
        padding: 1rem;
        margin-bottom: 0.8rem;

        &__label {
        display: block;
        font-size: 0.875rem;
        font-weight: 500;
        color: #374151;
        margin-bottom: 0.5rem;
        }
        &__input {
        width: 95%;
        padding: 0.5rem 1rem;
        border: 1px solid #d1d5db;
        border-radius: 0.5rem;
        font-size: 1rem;
        transition: all 0.2s ease;
        &:focus {
        outline: none;
        border-color: #6366f1;
        box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
        }
    }
  }



.converter-grid {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 50%;

    .converter-input-group,
    .converter-output-group {
    display: flex;
    align-items: start;
    justify-content: space-between;
    flex-direction: column;
    width: 100%;
    height: 100%;
    
    .converter-input-group__label,
    .converter-output-group__label {
    font-size: 1rem;
    font-weight: 700;
    color: #2c5048;
    }

    .converter-input-group__value-input {
    width: 100%;
    padding: 0.75rem 1rem;
    font-size: 1.125rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    transition: all 0.2s ease;
    &:focus {
    outline: none;
    border-color: #6366f1;
    box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
    }
    &::placeholder {
    color: #9ca3af;
    }
    }

    .converter-input-group__unit-select,
    .converter-output-group__unit-select {
    width: 100%;
    padding: 0.75rem 1rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    background: white;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.2s ease;
    }

    .converter-input-group__unit-select:focus,
    .converter-output-group__unit-select:focus {
    outline: none;
    border-color: #6366f1;
    box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
    }
  }


    .converter-swap {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 20%;
    height: 100%;
    padding: 0.5rem 0;

        .converter-swap__button {
        padding: 0.75rem;
        background: #e0e7ff;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        transition: all 0.2s ease;
        &:hover {
        background: #c7d2fe;
        }
        &:focus {
        outline: 2px solid #6366f1;
        outline-offset: 2px;
        }

        .converter-swap__icon {
        width: 1.25rem;
        height: 1.25rem;
        color: #4f46e5;
        }
    }
    }


    .converter-output-group__result {
    position: relative;
    width: 100%;

    .converter-output-group__result-input {
    width: 100%;
    padding: 0.75rem 1rem;
    font-size: 1.125rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    background: #f9fafb;
    font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
    color: #111827;
    &:read-only {
    cursor: not-allowed;
    }
    }


    .converter-output-group__copy-button {
    position: absolute;
    right: 0.5rem;
    top: 50%;
    transform: translateY(-50%);
    padding: 0.5rem;
    background: transparent;
    border: none;
    border-radius: 0.375rem;
    cursor: pointer;
    transition: all 0.2s ease;
    &:hover {
    background: #e5e7eb;
    }
    &:focus {
    outline: 2px solid #6366f1;
    outline-offset: 2px;
    }

    .converter-output-group__copy-button--copied .converter-output-group__copy-icon {
    color: #10b981;
    }

    .converter-output-group__copy-icon {
    width: 1rem;
    height: 1rem;
    color: #4b5563;
    }
    }
    }
  }
}

@media (max-width: 668px) {
  .converter-section {
    padding: 2rem;
    border-radius: 1.5rem;

  .converter-grid {
    flex-direction: column;
     .converter-input-group,
    .converter-output-group  {
        height: 18vh;
    }

    .converter-swap {
        height: 10vh;
        margin-top: 10px;
    }
  }

}
}
</style>