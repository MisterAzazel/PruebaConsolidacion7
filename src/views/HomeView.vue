<template>
  <div class="pagina-cursos">
    <h1 class="titulo-cursos">Listado de Cursos</h1>
    <div class="contenedor-cursos">
      <CursoCard v-for="curso in cursos" :key="curso.id" :curso="curso" />
    </div>
  </div>
</template>

<script>
import CursoCard from '@/components/CursoCard.vue'; // Ajusta la ruta si es necesario

export default {
  name: 'HomeView',
  components: {
    CursoCard
  },
  computed: {
    cursos() {
      return this.$store.state.cursos;
    }
  }
}
</script>

<style scoped>
.pagina-cursos {
  text-align: center;
  padding: 20px;
}

.titulo-cursos {
  font-size: 2rem;
  margin-bottom: 20px;
}

.contenedor-cursos {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>