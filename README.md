# 📚 Biblioteca Vue — App CRUD

Aplicación web hecha con **Vue 3** que consume una **API REST** (json-server) para gestionar
una biblioteca. Permite **crear, leer, actualizar y borrar** libros (CRUD completo).

## 🗂️ Entidades (3 tablas)

La API maneja 3 entidades relacionadas:

- **autores** (id, nombre, nacionalidad)
- **categorias** (id, nombre)
- **libros** (id, titulo, anio, autorId, categoriaId)

Cada **libro** se relaciona con un **autor** (`autorId`) y una **categoría** (`categoriaId`).

## ▶️ Cómo ejecutar el proyecto

1. Instalar dependencias:
   ```
   npm install
   ```
2. Levantar la API (json-server) — **Terminal 1**:
   ```
   npm run api
   ```
   Queda en `http://localhost:3000`
3. Levantar la app (Vite) — **Terminal 2**:
   ```
   npm run dev
   ```
   Abrir el link que muestra Vite (ej. `http://localhost:5173`)

> ⚠️ Se necesitan las **dos** terminales corriendo a la vez: la API sirve los datos y Vite sirve la app.

## ⚙️ Operaciones CRUD

| Operación | Acción en la app | Método HTTP |
|-----------|------------------|-------------|
| **Create** | Formulario "Agregar libro" | `POST /libros` |
| **Read**   | Tabla de libros (al cargar) | `GET /libros` |
| **Update** | Botón "Editar" | `PUT /libros/:id` |
| **Delete** | Botón "Borrar" | `DELETE /libros/:id` |

## 🧩 Conceptos de Vue usados (y dónde están)

| Concepto | Dónde |
|----------|-------|
| **Componentes** | `App.vue` (padre) y `components/LibroForm.vue` (hijo) |
| **Props** (padre → hijo) | `LibroForm` recibe `libroEditar`, `autores`, `categorias` |
| **Emit** (hijo → padre) | `LibroForm` emite `guardar` y `cancelar` |
| **v-model** (binding doble) | inputs y selects del formulario |
| **Binding de atributo** (`:`) | `:libroEditar`, `:value`, `:key` |
| **Interpolación** (`{{ }}`) | títulos, celdas de la tabla |
| **v-for** | filas de la tabla y opciones de los selects |
| **v-if / v-else-if / v-else** | "Cargando", tabla, "No se encontraron libros" |
| **computed** | `librosFiltrados` (buscador) |
| **methods** | `guardarLibro`, `borrarLibro`, `nombreAutor`, etc. |
| **watch** | vigila `libroEditar` para rellenar el formulario |
| **Hook `mounted`** | pide las 3 tablas a la API al iniciar |

## 🛠️ Tecnologías

- Vue 3
- Vite
- json-server (API REST de prueba)
