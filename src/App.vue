<template>
  <div>
    <!-- Barra de navegación. Le paso la vista actual y escucho cuándo cambia -->
    <NavBar :vistaActual="vista" @cambiar="cambiarVista" />

    <main>
      <!-- Según la vista elegida, muestro una página u otra -->
      <InicioView v-if="vista === 'inicio'" />
      <LibrosView v-else-if="vista === 'libros'" />
      <AutoresView v-else-if="vista === 'autores'" />
      <CategoriasView v-else-if="vista === 'categorias'" />
    </main>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import InicioView from './views/InicioView.vue'
import LibrosView from './views/LibrosView.vue'
import AutoresView from './views/AutoresView.vue'
import CategoriasView from './views/CategoriasView.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    InicioView,
    LibrosView,
    AutoresView,
    CategoriasView
  },
  data() {
    return {
      vista: 'inicio' // página que se muestra al inicio
    }
  },
  methods: {
    // El navbar me avisa (emit) qué página quiere y yo cambio la vista
    cambiarVista(nueva) {
      this.vista = nueva
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  min-height: 100vh;
  background: linear-gradient(135deg, #e0eafc 0%, #f3f6fb 100%);
  font-family: "Segoe UI", system-ui, -apple-system, Arial, sans-serif;
  color: #2c3e50;
}
.contenedor {
  max-width: 880px;
  margin: 32px auto;
  background: #ffffff;
  padding: 32px;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
}
.encabezado {
  text-align: center;
  margin-bottom: 24px;
}
.encabezado h1 {
  margin: 0;
  font-size: 2rem;
}
.subtitulo {
  margin: 4px 0 0;
  color: #7f8c8d;
}
.barra-busqueda {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 18px;
}
.buscador {
  flex: 1;
  padding: 12px 14px;
  border: 1px solid #dcdfe3;
  border-radius: 10px;
  font-size: 0.95rem;
}
.buscador:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.15);
}
.contador {
  background: #42b883;
  color: #fff;
  padding: 7px 14px;
  border-radius: 20px;
  font-size: 0.85rem;
  white-space: nowrap;
}
table {
  width: 100%;
  border-collapse: collapse;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}
thead th {
  background: #42b883;
  color: #fff;
  padding: 12px;
  text-align: left;
  font-weight: 600;
}
tbody td {
  padding: 12px;
  border-bottom: 1px solid #eee;
}
tbody tr:nth-child(even) {
  background: #f9fafb;
}
tbody tr:hover {
  background: #f0fdf8;
}
.etiqueta {
  background: #eaf6ff;
  color: #2980b9;
  padding: 3px 10px;
  border-radius: 12px;
  font-size: 0.82rem;
}
button {
  cursor: pointer;
  border: none;
  border-radius: 8px;
  padding: 6px 14px;
  font-size: 0.85rem;
  margin-right: 6px;
  color: #fff;
  transition: 0.15s;
}
.editar {
  background: #3498db;
}
.editar:hover {
  background: #2980b9;
}
.borrar {
  background: #e74c3c;
}
.borrar:hover {
  background: #c0392b;
}
.mensaje-vacio {
  text-align: center;
  color: #7f8c8d;
  padding: 28px;
  font-size: 1.05rem;
}
.lista {
  list-style: none;
  padding: 0;
  margin: 16px 0 0;
}
.lista li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  background: #f9fafb;
  border: 1px solid #eef1f4;
  border-radius: 8px;
  margin-bottom: 8px;
  font-size: 0.95rem;
}
.lista small {
  color: #95a5a6;
}
.mini-borrar {
  background: transparent;
  color: #e74c3c;
  font-weight: bold;
  padding: 2px 8px;
  margin: 0;
}
.mini-borrar:hover {
  background: #fdecea;
}
</style>
