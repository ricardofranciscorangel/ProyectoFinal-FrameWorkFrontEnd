<template>
  <div class="contenedor">
    <header class="encabezado">
      <h1>Bienvenido a la Biblioteca</h1>
      <p class="subtitulo">Catálogo de libros disponibles</p>
    </header>

    <!-- Tarjetas de resumen (cuántos hay de cada cosa) -->
    <div class="stats">
      <div class="stat">
        <span class="stat-num">{{ libros.length }}</span>
        <span class="stat-label">Libros</span>
      </div>
      <div class="stat">
        <span class="stat-num">{{ autores.length }}</span>
        <span class="stat-label">Autores</span>
      </div>
      <div class="stat">
        <span class="stat-num">{{ categorias.length }}</span>
        <span class="stat-label">Categorías</span>
      </div>
    </div>

    <p v-if="cargando" class="mensaje-vacio">Cargando catálogo...</p>

    <!-- Cuadrícula de tarjetas de libros (SOLO LECTURA, sin editar/borrar) -->
    <div v-else-if="libros.length > 0" class="grid-libros">
      <article v-for="libro in libros" :key="libro.id" class="card-libro">
        <h3>{{ libro.titulo }}</h3>
        <p class="autor">Autor: {{ nombreAutor(libro.autorId) }}</p>
        <div class="card-pie">
          <span class="etiqueta">{{ nombreCategoria(libro.categoriaId) }}</span>
          <span class="anio">{{ libro.anio }}</span>
        </div>
      </article>
    </div>

    <p v-else class="mensaje-vacio">Aún no hay libros en el catálogo.</p>
  </div>
</template>

<script>
export default {
  name: 'InicioView',
  data() {
    return {
      libros: [],
      autores: [],
      categorias: [],
      cargando: true
    }
  },
  methods: {
    async obtenerLibros() {
      const respuesta = await fetch('http://localhost:3000/libros')
      this.libros = await respuesta.json()
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
      return autor ? autor.nombre : 'Desconocido'
    },
    nombreCategoria(id) {
      const cat = this.categorias.find(c => c.id === id)
      return cat ? cat.nombre : 'Sin categoría'
    }
  },
  // Espero a que lleguen las 3 tablas y luego quito el "Cargando"
  async mounted() {
    await Promise.all([
      this.obtenerLibros(),
      this.obtenerAutores(),
      this.obtenerCategorias()
    ])
    this.cargando = false
  }
}
</script>

<style scoped>
.stats {
  display: flex;
  gap: 16px;
  justify-content: center;
  margin-bottom: 28px;
  flex-wrap: wrap;
}
.stat {
  background: #42b883;
  color: #fff;
  border-radius: 12px;
  padding: 16px 28px;
  text-align: center;
  min-width: 90px;
}
.stat-num {
  display: block;
  font-size: 1.8rem;
  font-weight: 700;
}
.stat-label {
  font-size: 0.85rem;
  opacity: 0.9;
}
.grid-libros {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
.card-libro {
  background: #fff;
  border: 1px solid #eef1f4;
  border-radius: 12px;
  padding: 18px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: 0.15s;
}
.card-libro:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}
.card-libro h3 {
  margin: 0 0 8px;
  font-size: 1.05rem;
}
.card-libro .autor {
  margin: 0 0 14px;
  color: #7f8c8d;
  font-size: 0.9rem;
}
.card-pie {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.anio {
  color: #95a5a6;
  font-size: 0.85rem;
  font-weight: 600;
}
</style>
