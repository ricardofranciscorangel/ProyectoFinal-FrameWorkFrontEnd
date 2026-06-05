<template>
  <form class="formulario" @submit.prevent="enviar">
    <!-- El título cambia según si estoy creando o editando -->
    <h2>{{ id ? '✏️ Editar libro' : '➕ Agregar libro' }}</h2>

    <!-- v-model = binding de doble vía: el input y la variable van conectados -->
    <input v-model="titulo" type="text" placeholder="Título del libro" required />
    <input v-model.number="anio" type="number" placeholder="Año" required />

    <!-- El texto del botón también cambia -->
    <button type="submit">{{ id ? 'Actualizar' : 'Guardar' }}</button>

    <!-- Solo aparece el botón Cancelar cuando estoy editando -->
    <button v-if="id" type="button" class="cancelar" @click="cancelar">Cancelar</button>
  </form>
</template>

<script>
export default {
  name: 'LibroForm',
  // PROP: el padre me MANDA el libro a editar (o null si es uno nuevo)
  props: {
    libroEditar: {
      type: Object,
      default: null
    }
  },
  // Datos propios del formulario
  data() {
    return {
      id: null,      // si tiene valor = estoy editando; si es null = estoy creando
      titulo: '',
      anio: ''
    }
  },
  // VIGILO el prop: cuando el padre me manda un libro, relleno el formulario
  watch: {
    libroEditar(nuevo) {
      if (nuevo) {
        this.id = nuevo.id
        this.titulo = nuevo.titulo
        this.anio = nuevo.anio
      }
    }
  },
  methods: {
    enviar() {
      // Armo el libro. Si id es null, el padre sabrá que es nuevo (crear)
      const libro = {
        id: this.id,
        titulo: this.titulo,
        anio: this.anio
      }
      this.$emit('guardar', libro) // aviso al padre y le mando el libro
      this.limpiar()
    },
    cancelar() {
      this.$emit('cancelar') // aviso al padre que cancelé la edición
      this.limpiar()
    },
    limpiar() {
      this.id = null
      this.titulo = ''
      this.anio = ''
    }
  }
}
</script>

<style scoped>
.formulario {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
  align-items: center;
  flex-wrap: wrap;
}
.formulario h2 {
  width: 100%;
  margin: 0 0 8px;
}
.formulario input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.formulario button {
  padding: 8px 16px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.formulario button:hover {
  background: #369870;
}
.formulario button.cancelar {
  background: #888;
}
</style>
