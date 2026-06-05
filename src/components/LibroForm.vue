<template>
  <form class="formulario" @submit.prevent="enviar">
    <!-- El título cambia según si estoy creando o editando -->
    <h2>{{ id ? 'Editar libro' : 'Agregar libro' }}</h2>

    <!-- v-model = binding de doble vía -->
    <input v-model="titulo" type="text" placeholder="Título del libro" required />
    <input v-model.number="anio" type="number" placeholder="Año" required />

    <!-- SELECT de autores: recorro la lista de autores que me pasó el padre -->
    <select v-model="autorId" required>
      <option value="" disabled>Selec. Autor</option>
      <option v-for="autor in autores" :key="autor.id" :value="autor.id">
        {{ autor.nombre }}
      </option>
    </select>

    <!-- SELECT de categorías -->
    <select v-model="categoriaId" required>
      <option value="" disabled>Selec. Categoría</option>
      <option v-for="cat in categorias" :key="cat.id" :value="cat.id">
        {{ cat.nombre }}
      </option>
    </select>

    <button type="submit">{{ id ? 'Actualizar' : 'Guardar' }}</button>
    <button v-if="id" type="button" class="cancelar" @click="cancelar">Cancelar</button>
  </form>
</template>

<script>
export default {
  name: 'LibroForm',
  // PROPS: el padre me manda el libro a editar y las listas de autores y categorías
  props: {
    libroEditar: {
      type: Object,
      default: null
    },
    autores: {
      type: Array,
      default: () => []
    },
    categorias: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      id: null,
      titulo: '',
      anio: '',
      autorId: '',
      categoriaId: ''
    }
  },
  // VIGILO el prop: cuando el padre me manda un libro, relleno todo el formulario
  watch: {
    libroEditar(nuevo) {
      if (nuevo) {
        this.id = nuevo.id
        this.titulo = nuevo.titulo
        this.anio = nuevo.anio
        this.autorId = nuevo.autorId
        this.categoriaId = nuevo.categoriaId
      }
    }
  },
  methods: {
    enviar() {
      const libro = {
        id: this.id,
        titulo: this.titulo,
        anio: this.anio,
        autorId: this.autorId,
        categoriaId: this.categoriaId
      }
      this.$emit('guardar', libro)
      this.limpiar()
    },
    cancelar() {
      this.$emit('cancelar')
      this.limpiar()
    },
    limpiar() {
      this.id = null
      this.titulo = ''
      this.anio = ''
      this.autorId = ''
      this.categoriaId = ''
    }
  }
}
</script>

<style scoped>
.formulario {
  display: flex;
  gap: 10px;
  margin-bottom: 24px;
  align-items: center;
  flex-wrap: wrap;
  background: #f7f9fb;
  padding: 18px;
  border-radius: 12px;
  border: 1px solid #eef1f4;
}
.formulario h2 {
  width: 100%;
  margin: 0 0 6px;
  font-size: 1.1rem;
  color: #2c3e50;
}
.formulario input,
.formulario select {
  padding: 10px 12px;
  border: 1px solid #dcdfe3;
  border-radius: 8px;
  font-size: 0.9rem;
  flex: 1;
  min-width: 140px;
  background: #fff;
}
.formulario input:focus,
.formulario select:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.15);
}
.formulario button {
  padding: 10px 18px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: 0.15s;
}
.formulario button:hover {
  background: #369870;
}
.formulario button.cancelar {
  background: #95a5a6;
}
.formulario button.cancelar:hover {
  background: #7f8c8d;
}
</style>
