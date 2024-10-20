<template>
  <div class="admin">
    <h1>Administración de Cursos</h1>
    <button @click="mostrarModal = true" class="btn-agregar">Agregar Curso</button>

    <table class="tabla-cursos">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Inscritos</th>
          <th>Cupos</th>
          <th>Duración</th>
          <th>Costo</th>
          <th>Terminado</th>
          <th>Fecha</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="curso in cursos" :key="curso.id">
          <td>{{ curso.nombre }}</td>
          <td>{{ curso.inscritos }}</td>
          <td>{{ curso.cupos }}</td>
          <td>{{ curso.duracion }}</td>
          <td>{{ curso.costo }}</td>
          <td>{{ curso.completado ? 'Sí' : 'No' }}</td>
          <td>{{ curso.fecha_registro }}</td>
          <td>
            <button @click="eliminarCurso(curso.id)" class="btn-eliminar">Eliminar</button>
            <button @click="editarCurso(curso)" class="btn-editar">Editar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="stats">
      <div class="stat-box">
        <h3>Total de Cupos</h3>
        <p>{{ totalCupos }}</p>
      </div>
      <div class="stat-box">
        <h3>Total de Inscritos</h3>
        <p>{{ totalInscritos }}</p>
      </div>
      <div class="stat-box">
        <h3>Cupos Restantes</h3>
        <p>{{ cuposRestantes }}</p>
      </div>
      <div class="stat-box">
        <h3>Cursos Terminados</h3>
        <p>{{ cursosTerminados }}</p>
      </div>
      <div class="stat-box">
        <h3>Cursos Activos</h3>
        <p>{{ cursosActivos }}</p>
      </div>
      <div class="stat-box">
        <h3>Total de Cursos</h3>
        <p>{{ totalCursos }}</p>
      </div>
    </div>

    <div v-if="mostrarModal" class="modal">
      <div class="contenido-modal">
        <h2>Agregar Nuevo Curso</h2>
        <form @submit.prevent="agregarNuevoCurso" class="formulario-modal">
          <label for="nombre">Nombre:</label>
          <input v-model="nuevoCurso.nombre" id="nombre" type="text" placeholder="Nombre del curso" />
          <p v-if="errores.nombre" class="error">{{ errores.nombre }}</p>

          <label for="img">Imagen (URL):</label>
          <input v-model="nuevoCurso.img" id="img" type="text" placeholder="URL de la imagen" />
          <p v-if="errores.img" class="error">{{ errores.img }}</p>

          <label for="cupos">Cupos:</label>
          <input v-model="nuevoCurso.cupos" id="cupos" type="number" placeholder="Cupos disponibles" />
          <p v-if="errores.cupos" class="error">{{ errores.cupos }}</p>

          <label for="inscritos">Inscritos:</label>
          <input v-model="nuevoCurso.inscritos" id="inscritos" type="number" placeholder="Número de inscritos" />
          <p v-if="errores.inscritos" class="error">{{ errores.inscritos }}</p>

          <label for="costo">Costo:</label>
          <input v-model="nuevoCurso.costo" id="costo" type="number" placeholder="Costo del curso" />
          <p v-if="errores.costo" class="error">{{ errores.costo }}</p>

          <label for="descripcion">Descripción:</label>
          <textarea v-model="nuevoCurso.descripcion" id="descripcion" placeholder="Descripción del curso"></textarea>
          <p v-if="errores.descripcion" class="error">{{ errores.descripcion }}</p>

          <label for="duracion">Duración:</label>
          <input v-model="nuevoCurso.duracion" id="duracion" type="text" placeholder="Duración del curso" />
          <p v-if="errores.duracion" class="error">{{ errores.duracion }}</p>

          <label for="fecha_registro">Fecha de registro:</label>
          <input v-model="nuevoCurso.fecha_registro" id="fecha_registro" type="text" disabled />

          <button type="submit" class="btn-agregar-modal">Agregar Curso</button>
          <button @click="mostrarModal = false" class="btn-cancelar-modal">Cancelar</button>
        </form>
      </div>
    </div>

    <div v-if="mostrarModalEditar" class="modal">
      <div class="contenido-modal">
        <h2>Editar Curso: {{ cursoSeleccionado.nombre }}</h2>
        <form @submit.prevent="guardarCambiosCurso" class="formulario-modal">
          <label for="nombre">Nombre:</label>
          <input v-model="cursoSeleccionado.nombre" id="nombre" type="text" />
          <p v-if="errores.nombre" class="error">{{ errores.nombre }}</p>

          <label for="img">Imagen (URL):</label>
          <input v-model="cursoSeleccionado.img" id="img" type="text" placeholder="URL de la imagen" />
          <p v-if="errores.img" class="error">{{ errores.img }}</p>

          <label for="costo">Costo:</label>
          <input v-model="cursoSeleccionado.costo" id="costo" type="number" />
          <p v-if="errores.costo" class="error">{{ errores.costo }}</p>

          <label for="duracion">Duración:</label>
          <input v-model="cursoSeleccionado.duracion" id="duracion" type="text" />
          <p v-if="errores.duracion" class="error">{{ errores.duracion }}</p>

          <label for="cupos">Cupos:</label>
          <input v-model="cursoSeleccionado.cupos" id="cupos" type="number" />
          <p v-if="errores.cupos" class="error">{{ errores.cupos }}</p>

          <label for="inscritos">Inscritos:</label>
          <input v-model="cursoSeleccionado.inscritos" id="inscritos" type="number" />
          <p v-if="errores.inscritos" class="error">{{ errores.inscritos }}</p>

          <label for="descripcion">Descripción:</label>
          <textarea v-model="cursoSeleccionado.descripcion" id="descripcion"></textarea>
          <p v-if="errores.descripcion" class="error">{{ errores.descripcion }}</p>

          <label for="completado">Terminado:</label>
          <textarea v-model="cursoSeleccionado.completado" id="completado"></textarea>

          <label for="fecha">Fecha:</label>
          <textarea v-model="cursoSeleccionado.fecha_registro" id="fecha"></textarea>

          <button type="submit" class="btn-guardar-modal">Guardar Cambios</button>
          <button @click="cerrarModalEditar" class="btn-cancelar-modal">Cancelar</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mostrarModal: false,
      mostrarModalEditar: false,
      nuevoCurso: {
        nombre: '',
        img: '',
        cupos: 0,
        inscritos: 0,
        costo: 0,
        descripcion: '',
        duracion: '',
        completado: false,
        fecha_registro: new Date().toLocaleDateString(),
      },
      errores: {
        nombre: '',
        img: '',
        cupos: '',
        inscritos: '',
        costo: '',
        descripcion: '',
        duracion: ''
      },
      cursoSeleccionado: null,
    };
  },

  computed: {
    cursos() {
      return this.$store.state.cursos;
    },
    totalCupos() {
      return this.cursos.reduce((total, curso) => total + Number(curso.cupos), 0);
    },
    totalInscritos() {
      return this.cursos.reduce((total, curso) => total + Number(curso.inscritos), 0);
    },
    cuposRestantes() {
      return this.totalCupos - this.totalInscritos;
    },
    cursosTerminados() {
      return this.cursos.filter(curso => curso.completado).length;
    },
    cursosActivos() {
      return this.cursos.filter(curso => !curso.completado).length;
    },
    totalCursos() {
      return this.cursos.length;
    }
  },
  methods: {
    agregarNuevoCurso() {
      this.errores = {
        nombre: '',
        img: '',
        cupos: '',
        inscritos: '',
        costo: '',
        descripcion: '',
        duracion: ''
      };

      // Validaciones
      if (this.nuevoCurso.inscritos > this.nuevoCurso.cupos) {
        this.errores.inscritos = 'Los inscritos no pueden ser mayores a los cupos.';
        return;
      }

      if (!this.nuevoCurso.nombre) {
        this.errores.nombre = 'El nombre es obligatorio.';
        return;
      }

      if (!this.nuevoCurso.img) {
        this.errores.img = 'La imagen es obligatoria.';
        return;
      }

      if (this.nuevoCurso.cupos <= 0) {
        this.errores.cupos = 'Debe haber al menos un cupo disponible.';
        return;
      }

      if (this.nuevoCurso.inscritos < 0) {
        this.errores.inscritos = 'Los inscritos no pueden ser un valor negativo.';
        return;
      }

      if (this.nuevoCurso.costo <= 0) {
        this.errores.costo = 'El costo debe ser mayor a 0.';
        return;
      }

      if (!this.nuevoCurso.descripcion) {
        this.errores.descripcion = 'La descripción es obligatoria.';
        return;
      }

      if (!this.nuevoCurso.duracion) {
        this.errores.duracion = 'La duración es obligatoria.';
        return;
      }

      this.$store.dispatch('agregarCurso', { ...this.nuevoCurso, id: Date.now() });

      this.mostrarModal = false;

      this.nuevoCurso = {
        nombre: '',
        img: '',
        cupos: 0,
        inscritos: 0,
        costo: 0,
        descripcion: '',
        duracion: '',
        completado: false,
        fecha_registro: new Date().toLocaleDateString(),
      };
    },

    eliminarCurso(cursoId) {
      this.$store.dispatch('eliminarCurso', cursoId);
    },
    editarCurso(curso) {
      this.cursoSeleccionado = { ...curso };
      this.mostrarModalEditar = true;
    },
    guardarCambiosCurso() {
      if (this.cursoSeleccionado.inscritos > this.cursoSeleccionado.cupos) {
        alert('Los inscritos no pueden ser mayores a los cupos.');
        return;
      }
      this.$store.dispatch('actualizarCurso', this.cursoSeleccionado);
      this.cerrarModalEditar();
    },
    cerrarModalEditar() {
      this.mostrarModalEditar = false;
      this.cursoSeleccionado = null;
    }
  }
};
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f4;
  color: #333;
  margin: 0;
  padding: 0;
}

.admin {
  max-width: 1200px;
  margin: 20px auto;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #4CAF50;
}

.btn-agregar, .btn-editar, .btn-eliminar {
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 15px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-agregar:hover, .btn-editar:hover, .btn-eliminar:hover {
  background-color: #45a049;
}

.tabla-cursos {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

.tabla-cursos th, .tabla-cursos td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

.tabla-cursos th {
  background-color: #f2f2f2;
  color: #333;
}

.estadisticas {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin: 20px 0;
}

.caja-estadistica {
  background: #e7f4e4;
  border-radius: 8px;
  padding: 15px;
  flex: 1 1 30%;
  margin: 10px;
  text-align: center;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.contenido-modal {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 90%;
  max-width: 600px;
  min-height: 400px;
  max-height: 80vh;
  overflow-y: auto;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.formulario-modal {
  display: flex;
  flex-direction: column;
}

.formulario-modal label {
  margin-top: 10px;
}

.formulario-modal input,
.formulario-modal textarea {
  margin-bottom: 15px;
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn-guardar-modal,
.btn-agregar-modal,
.btn-cancelar-modal {
  padding: 10px 15px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

.btn-guardar-modal, .btn-agregar-modal {
  background-color: #007bff;
  color: white;
}

.btn-cancelar-modal {
  background-color: #dc3545;
  color: white;
}

.btn-guardar-modal:hover,
.btn-agregar-modal:hover,
.btn-cancelar-modal:hover {
  opacity: 0.9;
}

.stats {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.stat-box {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  text-align: center;
  transition: transform 0.2s;
}

.stat-box:hover {
  transform: translateY(-5px);
}

.stat-box h3 {
  margin: 0 0 10px;
  font-size: 1.2em;
  color: #333;
}

.stat-box p {
  font-size: 1.5em;
  color: #4CAF50;
  font-weight: bold;
}

.error {
  color: red;
  font-size: 12px;
  margin-top: -10px;
  margin-bottom: 10px;
}
</style>