<template>
    <div class="tarjeta-curso">
      <img class="imagen-curso" :src="curso.img" alt="Imagen del curso" />
      <h2 class="nombre-curso">{{ curso.nombre }}</h2>
      <p class="costo-curso">Costo: {{ curso.costo }}</p>
      <p class="duracion-curso">Duración: {{ curso.duracion }}</p>
      <p class="cupos-curso">Cupos: {{ curso.cupos }}</p>
      <p class="completado-curso">Completado: {{ curso.completado }}</p>
      <p class="fecha-registro-curso">Fecha de Registro: {{ curso.fecha_registro }}</p>
      <p class="descripcion-curso">{{ curso.descripcion }}</p>
    </div>
  </template>

  <script>
  export default {
    name: 'CursoCard',
    props: {
      curso: {
        type: Object,
        required: true
      }
    }
  }
  </script>

  <style scoped>
  .tarjeta-curso {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 16px;
    margin: 10px;
    max-width: 300px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s;
  }

  .tarjeta-curso:hover {
    transform: scale(1.05);
  }

  .imagen-curso {
    max-width: 100%;
    height: auto;
    border-radius: 4px;
  }

  .nombre-curso {
    font-size: 1.5rem;
    margin: 10px 0;
  }

  .costo-curso,
  .duracion-curso,
  .cupos-curso,
  .completado-curso,
  .fecha-registro-curso {
    margin: 5px 0;
    color: #555;
  }

  .descripcion-curso {
    font-size: 0.9rem;
    color: #777;
  }
  </style>