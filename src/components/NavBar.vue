<template>
  <nav class="navbar">
    <span class="logo">📚 Biblioteca</span>
    <div class="links">
      <!-- :class dinámico: pone la clase "activo" solo en el botón de la vista actual -->
      <button
        :class="{ activo: vistaActual === 'libros' }"
        @click="$emit('cambiar', 'libros')"
      >
        Libros
      </button>
      <button
        :class="{ activo: vistaActual === 'autores' }"
        @click="$emit('cambiar', 'autores')"
      >
        Autores
      </button>
      <button
        :class="{ activo: vistaActual === 'categorias' }"
        @click="$emit('cambiar', 'categorias')"
      >
        Categorías
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  // Recibe del padre cuál vista está activa (para resaltar el botón)
  props: {
    vistaActual: {
      type: String,
      default: 'libros'
    }
  }
}
</script>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #2c3e50;
  padding: 0 24px;
  height: 60px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}
.logo {
  color: #fff;
  font-size: 1.2rem;
  font-weight: 700;
}
.links {
  display: flex;
  gap: 8px;
}
.links button {
  background: transparent;
  color: #cfd8e3;
  border: none;
  padding: 10px 16px;
  font-size: 0.95rem;
  cursor: pointer;
  border-radius: 8px;
  transition: 0.15s;
  margin: 0;
}
.links button:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}
.links button.activo {
  background: #42b883;
  color: #fff;
}
</style>
