<template>
  <div class="contenedor">
    <h1>📚 Biblioteca</h1>

    <!-- Mientras carga, muestra esto -->
    <p v-if="cargando">Cargando libros...</p>

    <!-- Si ya cargó y SÍ hay libros, muestra la tabla -->
    <table v-else-if="libros.length > 0">
      <thead>
        <tr>
          <th>ID</th>
          <th>Título</th>
          <th>Año</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="libro in libros" :key="libro.id">
          <td>{{ libro.id }}</td>
          <td>{{ libro.titulo }}</td>
          <td>{{ libro.anio }}</td>
        </tr>
      </tbody>
    </table>

    <!-- Si ya cargó pero NO hay libros -->
    <p v-else>No hay libros para mostrar.</p>
  </div>
</template>

<script>
export default {
  name: 'App',
  // 1) Aquí guardo los datos del componente
  data() {
    return {
      libros: [],      // aquí guardaré los libros que vengan de la API
      cargando: true   // bandera para saber si todavía está cargando
    }
  },
  // 2) Aquí van mis funciones
  methods: {
    async obtenerLibros() {
      const respuesta = await fetch('http://localhost:3000/libros')
      this.libros = await respuesta.json()  // guardo los libros en mi data
      this.cargando = false                 // ya terminó de cargar
    }
  },
  // 3) Hook: se ejecuta solito cuando el componente aparece en pantalla
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
</style>
