<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>📚 Libros</h1>
      <p class="subtitulo">Gestiona tu colección de libros</p>
    </header>

    <!-- Formulario de libros (crear / editar) -->
    <LibroForm
      :libroEditar="libroSeleccionado"
      :autores="autores"
      :categorias="categorias"
      @guardar="guardarLibro"
      @cancelar="cancelarEdicion"
    />

    <!-- Búsqueda + contador -->
    <div class="barra-busqueda">
      <input
        class="buscador"
        v-model="busqueda"
        type="text"
        placeholder="🔍 Buscar por título..."
      />
      <span class="contador">{{ librosFiltrados.length }} libro(s)</span>
    </div>

    <p v-if="cargando" class="mensaje-vacio">Cargando libros...</p>

    <table v-else-if="librosFiltrados.length > 0">
      <thead>
        <tr>
          <th>ID</th>
          <th>Título</th>
          <th>Año</th>
          <th>Autor</th>
          <th>Categoría</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="libro in librosFiltrados" :key="libro.id">
          <td>{{ libro.id }}</td>
          <td>{{ libro.titulo }}</td>
          <td>{{ libro.anio }}</td>
          <td>{{ nombreAutor(libro.autorId) }}</td>
          <td>
            <span class="etiqueta">{{ nombreCategoria(libro.categoriaId) }}</span>
          </td>
          <td>
            <button class="editar" @click="seleccionarLibro(libro)">Editar</button>
            <button class="borrar" @click="borrarLibro(libro.id)">Borrar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <p v-else class="mensaje-vacio">📭 No se encontraron libros.</p>
  </div>
</template>

<script>
import LibroForm from '../components/LibroForm.vue'

export default {
  name: 'LibrosView',
  components: { LibroForm },
  data() {
    return {
      libros: [],
      autores: [],
      categorias: [],
      cargando: true,
      libroSeleccionado: null,
      busqueda: ''
    }
  },
  computed: {
    librosFiltrados() {
      const texto = this.busqueda.toLowerCase()
      return this.libros.filter(libro =>
        libro.titulo.toLowerCase().includes(texto)
      )
    }
  },
  methods: {
    async obtenerLibros() {
      const respuesta = await fetch('http://localhost:3000/libros')
      this.libros = await respuesta.json()
      this.cargando = false
    },
    async obtenerAutores() {
      const respuesta = await fetch('http://localhost:3000/autores')
      this.autores = await respuesta.json()
    },
    async obtenerCategorias() {
      const respuesta = await fetch('http://localhost:3000/categorias')
      this.categorias = await respuesta.json()
    },
    nombreAutor(id) {
      const autor = this.autores.find(a => a.id === id)
      return autor ? autor.nombre : '—'
    },
    nombreCategoria(id) {
      const cat = this.categorias.find(c => c.id === id)
      return cat ? cat.nombre : '—'
    },
    seleccionarLibro(libro) {
      this.libroSeleccionado = { ...libro }
    },
    cancelarEdicion() {
      this.libroSeleccionado = null
    },
    async guardarLibro(libro) {
      if (libro.id) {
        await fetch(`http://localhost:3000/libros/${libro.id}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(libro)
        })
      } else {
        await fetch('http://localhost:3000/libros', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(libro)
        })
      }
      this.libroSeleccionado = null
      this.obtenerLibros()
    },
    async borrarLibro(id) {
      if (!confirm('¿Seguro que quieres borrar este libro?')) return
      await fetch(`http://localhost:3000/libros/${id}`, { method: 'DELETE' })
      this.obtenerLibros()
    }
  },
  // Al entrar a esta página, pido los datos que necesito
  mounted() {
    this.obtenerLibros()
    this.obtenerAutores()
    this.obtenerCategorias()
  }
}
</script>
