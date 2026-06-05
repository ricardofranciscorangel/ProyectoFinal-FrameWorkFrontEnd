<template>
  <form class="mini-form" @submit.prevent="enviar">
    <input v-model="nombre" type="text" placeholder="Nombre del autor" required />
    <input v-model="nacionalidad" type="text" placeholder="Nacionalidad" required />
    <input v-model.number="anioNacimiento" type="number" placeholder="Año de nacimiento" required />
    <button type="submit">{{ id ? 'Actualizar' : 'Agregar' }}</button>
    <button v-if="id" type="button" class="cancelar" @click="cancelar">Cancelar</button>
  </form>
</template>

<script>
export default {
  name: 'AutorForm',
  // PROP: el padre me manda el autor a editar (o null si es nuevo)
  props: {
    autorEditar: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      id: null, // si tiene valor = editando; si es null = creando
      nombre: '',
      nacionalidad: '',
      anioNacimiento: ''
    }
  },
  // Cuando el padre me pasa un autor, relleno el formulario
  watch: {
    autorEditar(nuevo) {
      if (nuevo) {
        this.id = nuevo.id
        this.nombre = nuevo.nombre
        this.nacionalidad = nuevo.nacionalidad
        this.anioNacimiento = nuevo.anioNacimiento
      }
    }
  },
  methods: {
    enviar() {
      this.$emit('guardar', {
        id: this.id,
        nombre: this.nombre,
        nacionalidad: this.nacionalidad,
        anioNacimiento: this.anioNacimiento
      })
      this.limpiar()
    },
    cancelar() {
      this.$emit('cancelar')
      this.limpiar()
    },
    limpiar() {
      this.id = null
      this.nombre = ''
      this.nacionalidad = ''
      this.anioNacimiento = ''
    }
  }
}
</script>

<style scoped>
.mini-form {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
.mini-form input {
  flex: 1;
  min-width: 100px;
  padding: 8px 10px;
  border: 1px solid #dcdfe3;
  border-radius: 8px;
  font-size: 0.85rem;
}
.mini-form input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.15);
}
.mini-form button {
  padding: 8px 14px;
  background: #42b883;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
}
.mini-form button:hover {
  background: #369870;
}
.mini-form button.cancelar {
  background: #95a5a6;
}
.mini-form button.cancelar:hover {
  background: #7f8c8d;
}
</style>
