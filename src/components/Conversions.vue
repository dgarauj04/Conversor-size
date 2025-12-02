<template>
  <section class="conversions-section">
    <h2 class="conversions-section__title">Todas as conversões</h2>
    <div class="conversions-grid">
      <div
        v-for="conversion in conversions"
        :key="conversion.unit"
        @click="emit('copyConversion', conversion)"
        class="conversion-card"
        :title="`Copiar ${conversion.value}${conversion.unit}`"
      >
        <span class="conversion-card__unit">{{ conversion.unit }}</span>
        <span class="conversion-card__value">
          {{ conversion.value || '0' }}{{ conversion.unit }}
        </span>
      </div>
    </div>
    <p class="conversions-section__hint">
      Clique em qualquer valor para copiar
    </p>
  </section>
</template>

<script setup>
const props = defineProps({
  conversions: {
    type: Array,
    required: true,
    default: () => []
  }
})

const emit = defineEmits(['copyConversion'])
</script>

<style scoped>
.conversions-section {
  background: #ffffff8b;
  border-radius: 1rem;
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  width: 100%;
  margin-bottom: 1.5rem;
}

.conversions-section__title {
  font-size: 1.125rem;
  font-weight: 600;
  color: #374151;
  margin-bottom: 1rem;
}

.conversions-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 0.75rem;
  margin-bottom: 1rem;
}

.conversion-card {
  padding: 1rem;
  background: linear-gradient(135deg, #eaeaea 0%, #bfe7cee3 100%);
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.conversion-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.conversion-card:active {
  transform: translateY(0);
}

.conversion-card__unit {
  display: block;
  font-size: 0.875rem;
  color: #4b5563;
  margin-bottom: 0.25rem;
}

.conversion-card__value {
  display: block;
  font-size: 1.25rem;
  font-weight: 600;
  font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
  color: #111827;
}

.conversions-section__hint {
  font-size: 0.85rem;
  font-weight: 500;
  color: #4d2eaa;
  text-align: center;
  margin-top: 1rem;
}

@media (min-width: 640px) {
  .conversions-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .conversions-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 768px) {
  .conversions-section {
    padding: 2rem;
    border-radius: 1.5rem;
  }
}
</style>