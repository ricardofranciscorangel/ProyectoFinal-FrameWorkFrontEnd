<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>🏷️ Categorías</h1>
      <p class="subtitulo">Administra las categorías de la biblioteca</p>
    </header>

    <!-- Formulario para agregar una categoría -->
    <CategoriaForm @guardar="crearCategoria" />

    <p v-if="categorias.length === 0" class="mensaje-vacio">No hay categorías registradas.</p>

    <ul v-else class="lista">
      <li v-for="cat in categorias" :key="cat.id">
        <span>{{ cat.nombre }}</span>
        <button class="mini-borrar" @click="borrarCategoria(cat.id)">✕</button>
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
      categorias: []
    }
  },
  methods: {
    async obtenerCategorias() {
      const respuesta = await fetch('http://localhost:3000/categorias')
      this.categorias = await respuesta.json()
    },
    async crearCategoria(categoria) {
      await fetch('http://localhost:3000/categorias', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(categoria)
      })
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
