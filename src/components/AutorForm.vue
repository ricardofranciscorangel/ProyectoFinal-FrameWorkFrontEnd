<template>
  <form class="mini-form" @submit.prevent="enviar">
    <input v-model="nombre" type="text" placeholder="Nombre del autor" required />
    <input v-model="nacionalidad" type="text" placeholder="Nacionalidad" required />
    <button type="submit">Agregar</button>
  </form>
</template>

<script>
export default {
  name: 'AutorForm',
  data() {
    return {
      nombre: '',
      nacionalidad: ''
    }
  },
  methods: {
    enviar() {
      // Aviso al padre con el autor nuevo (emit, igual que en LibroForm)
      this.$emit('guardar', {
        nombre: this.nombre,
        nacionalidad: this.nacionalidad
      })
      this.nombre = ''
      this.nacionalidad = ''
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
</style>
