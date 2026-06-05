<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>Autores</h1>
      <p class="subtitulo">Administra los autores de la biblioteca</p>
    </header>

    <!-- Formulario: le paso el autor a editar y escucho guardar/cancelar -->
    <AutorForm
      :autorEditar="autorSeleccionado"
      @guardar="guardarAutor"
      @cancelar="cancelarEdicion"
    />

    <p v-if="autores.length === 0" class="mensaje-vacio">No hay autores registrados.</p>

    <ul v-else class="lista">
      <li v-for="autor in autores" :key="autor.id">
        <span>
          {{ autor.nombre }}
          <small>
            ({{ autor.nacionalidad }}<span v-if="autor.anioNacimiento">, n. {{ autor.anioNacimiento }}</span>)
          </small>
        </span>
        <span class="acciones-lista">
          <button class="editar" @click="seleccionarAutor(autor)">Editar</button>
          <button class="borrar" @click="borrarAutor(autor.id)">Borrar</button>
        </span>
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
      autores: [],
      autorSeleccionado: null // el autor que estoy editando (null = ninguno)
    }
  },
  methods: {
    async obtenerAutores() {
      const respuesta = await fetch('http://localhost:3000/autores')
      this.autores = await respuesta.json()
    },
    seleccionarAutor(autor) {
      this.autorSeleccionado = { ...autor } // copia para el formulario
    },
    cancelarEdicion() {
      this.autorSeleccionado = null
    },
    // Si trae id -> ACTUALIZA (PUT); si no -> CREA (POST)
    async guardarAutor(autor) {
      if (autor.id) {
        await fetch(`http://localhost:3000/autores/${autor.id}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(autor)
        })
      } else {
        await fetch('http://localhost:3000/autores', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(autor)
        })
      }
      this.autorSeleccionado = null
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

<style scoped>
.acciones-lista {
  display: flex;
  align-items: center;
  gap: 4px;
}
</style>
