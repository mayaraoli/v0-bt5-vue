<template>
  <h2 class="curso-titulo">{{ titulo }}</h2>
</template>

<script>
export default {
  name: "CursoTitulo",
  props: {
    titulo: {
      type: String,
      required: true
    }
  }
};
</script>

<style scoped>
.curso-titulo {
  margin-top: 40px;
  text-align: center;
  font-size: 1.8rem;
  color: var(--bs-body-color);
  border-bottom: 2px solid var(--bs-border-color);
  padding-bottom: 8px;
}
</style>