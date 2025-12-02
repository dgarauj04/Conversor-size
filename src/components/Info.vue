<template>
  <aside class="tips-section">
    <h3 class="tips-section__title">Dicas:</h3>
    <ul class="tips-list">
      <li class="tips-list__item">
        <strong>rem/em:</strong> Baseados no tamanho da fonte (padrão: 16px)
      </li>
      <li class="tips-list__item">
        <strong>vw/vh:</strong> Relativos à largura/altura da viewport
      </li>
      <li class="tips-list__item">
        <strong>vmin/vmax:</strong> Baseados na menor/maior dimensão da viewport
      </li>
      <li class="tips-list__item">
        Use o ícone de configurações para ajustar o tamanho base da fonte
      </li>
    </ul>
  </aside>
</template>

<script setup>
</script>

<style scoped>
.tips-section {
  background: #ffffff8b;
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  padding: 1.25rem;
  width: 100%;
}

.tips-section__title {
  font-size: 0.95rem;
  font-weight: 600;
  color: #1d2737;
  margin-bottom: 0.75rem;
}

.tips-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.tips-list__item {
  font-size: 0.9rem;
  color: #4b5563;
  margin-bottom: 0.25rem;
  line-height: 1.4;
}

.tips-list__item:last-child {
  margin-bottom: 0;
}

.tips-list__item strong {
  color: #374151;
  font-weight: 600;
}
</style>