<template>
  <header class="converter-header">
    <h1 class="converter-title">Conversor de Unidades CSS</h1>
    <p class="converter-subtitle">Converta entre px, rem, em, vw, vh e outras unidades CSS</p>
  </header>
</template>

<script setup>
</script>

<style scoped>
.converter-header {
  width: 100%;
  height: 15vh;
  text-align: center;
  margin-bottom: 2.5rem;
}

.converter-title {
  font-size: 2rem;
  font-weight: 700;
  color: #1f2937;
  margin-bottom: 0.5rem;
  line-height: 1.2;
}

.converter-subtitle {
  color: #25124f;
  font-size: 1rem;
}

@media (min-width: 768px) {
  .converter-title {
    font-size: 3rem;
  }

  .converter-subtitle {
    font-size: 1.125rem;
  }
}
</style>