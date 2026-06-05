<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>Categorías</h1>
      <p class="subtitulo">Administra las categorías de la biblioteca</p>
    </header>

    <!-- Formulario: le paso la categoría a editar y escucho guardar/cancelar -->
    <CategoriaForm
      :categoriaEditar="categoriaSeleccionada"
      @guardar="guardarCategoria"
      @cancelar="cancelarEdicion"
    />

    <p v-if="categorias.length === 0" class="mensaje-vacio">No hay categorías registradas.</p>

    <ul v-else class="lista">
      <li v-for="cat in categorias" :key="cat.id">
        <span>{{ cat.nombre }}</span>
        <span class="acciones-lista">
          <button class="editar" @click="seleccionarCategoria(cat)">Editar</button>
          <button class="mini-borrar" @click="borrarCategoria(cat.id)">✕</button>
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
import CategoriaForm from '../components/CategoriaForm.vue'

export default {
  name: 'CategoriasView',
  components: { CategoriaForm },
  data() {
    return {
      categorias: [],
      categoriaSeleccionada: null
    }
  },
  methods: {
    async obtenerCategorias() {
      const respuesta = await fetch('http://localhost:3000/categorias')
      this.categorias = await respuesta.json()
    },
    seleccionarCategoria(cat) {
      this.categoriaSeleccionada = { ...cat }
    },
    cancelarEdicion() {
      this.categoriaSeleccionada = null
    },
    async guardarCategoria(categoria) {
      if (categoria.id) {
        await fetch(`http://localhost:3000/categorias/${categoria.id}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(categoria)
        })
      } else {
        await fetch('http://localhost:3000/categorias', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(categoria)
        })
      }
      this.categoriaSeleccionada = null
      this.obtenerCategorias()
    },
    async borrarCategoria(id) {
      if (!confirm('¿Borrar esta categoría?')) return
      await fetch(`http://localhost:3000/categorias/${id}`, { method: 'DELETE' })
      this.obtenerCategorias()
    }
  },
  mounted() {
    this.obtenerCategorias()
  }
}
</script>

<style scoped>
.acciones-lista {
  display: flex;
  align-items: center;
  gap: 4px;
}
</style>
