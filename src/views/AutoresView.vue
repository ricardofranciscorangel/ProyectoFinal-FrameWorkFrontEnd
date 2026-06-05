<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>✍️ Autores</h1>
      <p class="subtitulo">Administra los autores de la biblioteca</p>
    </header>

    <!-- Formulario para agregar un autor -->
    <AutorForm @guardar="crearAutor" />

    <p v-if="autores.length === 0" class="mensaje-vacio">No hay autores registrados.</p>

    <ul v-else class="lista">
      <li v-for="autor in autores" :key="autor.id">
        <span>{{ autor.nombre }} <small>({{ autor.nacionalidad }})</small></span>
        <button class="mini-borrar" @click="borrarAutor(autor.id)">✕</button>
      </li>
    </ul>
  </div>
</template>

<script>
import AutorForm from '../components/AutorForm.vue'

export default {
  name: 'AutoresView',
  components: { AutorForm },
  data() {
    return {
      autores: []
    }
  },
  methods: {
    async obtenerAutores() {
      const respuesta = await fetch('http://localhost:3000/autores')
      this.autores = await respuesta.json()
    },
    async crearAutor(autor) {
      await fetch('http://localhost:3000/autores', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(autor)
      })
      this.obtenerAutores()
    },
    async borrarAutor(id) {
      if (!confirm('¿Borrar este autor?')) return
      await fetch(`http://localhost:3000/autores/${id}`, { method: 'DELETE' })
      this.obtenerAutores()
    }
  },
  mounted() {
    this.obtenerAutores()
  }
}
</script>
