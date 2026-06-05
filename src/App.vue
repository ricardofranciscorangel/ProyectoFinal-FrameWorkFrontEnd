<template>
  <div class="contenedor">
    <h1>📚 Biblioteca</h1>

    <!-- Componente HIJO: el formulario.
         :libroEditar  -> le MANDO el libro a editar (prop, padre -> hijo)
         @guardar      -> escucho cuando el hijo guarda
         @cancelar     -> escucho cuando el hijo cancela -->
    <LibroForm
      :libroEditar="libroSeleccionado"
      @guardar="guardarLibro"
      @cancelar="cancelarEdicion"
    />

    <!-- Mientras carga, muestra esto -->
    <p v-if="cargando">Cargando libros...</p>

    <!-- Si ya cargó y SÍ hay libros, muestra la tabla -->
    <table v-else-if="libros.length > 0">
      <thead>
        <tr>
          <th>ID</th>
          <th>Título</th>
          <th>Año</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="libro in libros" :key="libro.id">
          <td>{{ libro.id }}</td>
          <td>{{ libro.titulo }}</td>
          <td>{{ libro.anio }}</td>
          <td>
            <button @click="seleccionarLibro(libro)">Editar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Si ya cargó pero NO hay libros -->
    <p v-else>No hay libros para mostrar.</p>
  </div>
</template>

<script>
// Importo el componente hijo para poder usarlo
import LibroForm from './components/LibroForm.vue'

export default {
  name: 'App',
  components: {
    LibroForm
  },
  data() {
    return {
      libros: [],
      cargando: true,
      libroSeleccionado: null // el libro que estoy editando (null = ninguno)
    }
  },
  methods: {
    async obtenerLibros() {
      const respuesta = await fetch('http://localhost:3000/libros')
      this.libros = await respuesta.json()
      this.cargando = false
    },
    // Cuando doy clic en "Editar": copio el libro y se lo paso al formulario
    seleccionarLibro(libro) {
      this.libroSeleccionado = { ...libro } // copia, para no tocar la tabla directo
    },
    cancelarEdicion() {
      this.libroSeleccionado = null
    },
    // Guarda el libro: si trae id -> ACTUALIZA (PUT); si no -> CREA (POST)
    async guardarLibro(libro) {
      if (libro.id) {
        // EDITAR (PUT)
        await fetch(`http://localhost:3000/libros/${libro.id}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(libro)
        })
      } else {
        // CREAR (POST)
        await fetch('http://localhost:3000/libros', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(libro)
        })
      }
      this.libroSeleccionado = null // limpio la selección
      this.obtenerLibros()          // recargo la lista
    }
  },
  mounted() {
    this.obtenerLibros()
  }
}
</script>

<style>
.contenedor {
  max-width: 700px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid #ccc;
  padding: 8px 12px;
  text-align: left;
}
th {
  background: #42b883;
  color: white;
}
button {
  cursor: pointer;
}
</style>
