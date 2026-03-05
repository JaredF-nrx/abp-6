<template>
  <header class="py-3">
    <h1 class="text-center">Vista Libros</h1>
  </header>

  <main class="container py-4">
    <section v-if="loginStore.currentUser.admin">
      <h2 class="text-center">Agregar un nuevo libro</h2>

      <form @submit.prevent="crearLibro">
        <div class="mb-2">
          <label class="form-label">Titulo:</label>
          <input type="text" name="titulo" class="form-control" required v-model="libro.titulo" />
        </div>
        <div class="mb-2">
          <label class="form-label">Autor:</label>
          <input type="text" name="autor" class="form-control" required v-model="libro.autor" />
        </div>
        <div class="mb-2">
          <label class="form-label">Categoría:</label>
          <select name="categoria" class="form-control" v-model="libro.categoria" required>
            <option value="">Elige una opción...</option>
            <option
              v-for="(categoria, index) in booksStore.categorias"
              :key="index"
              :value="categoria"
            >
              {{ categoria }}
            </option>
          </select>
        </div>
        <div class="py-2">
          <button type="submit" class="btn btn-primary">Crear</button>
        </div>
      </form>
    </section>

    <section>
      <h2 class="text-center">Listado de libros</h2>

      <div class="row">
        <div v-for="libro in booksStore.libros" :key="libro.id" class="col-md-4 mb-4">
          <div class="card h-100">
            <div class="card-body">
              <h5 class="card-title">{{ libro.titulo }}</h5>
              <p class="card-text">Autor: {{ libro.autor }}</p>
              <p class="card-text">Categoría: {{ libro.categoria }}</p>
              <router-link
                :to="{
                  name: 'book-detail',
                  params: {
                    id: libro.id,
                    titulo: libro.titulo,
                    categoria: libro.categoria,
                    autor: libro.autor,
                  },
                }"
                class="btn btn-primary me-2"
              >
                Ver detalle
              </router-link>
              <button
                v-if="loginStore.currentUser.admin"
                class="btn btn-danger"
                @click="eliminarLibro(libro.id)"
              >
                Eliminar
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import { useBooksStore } from '@/stores/books.store.js'
import { useLoginStore } from '@/stores/login.store.js'

const booksStore = useBooksStore()
const loginStore = useLoginStore()

const libro = ref({
  id: '',
  titulo: '',
  autor: '',
  categoria: '',
})

const resetLibro = () => {
  libro.value.id = ''
  libro.value.titulo = ''
  libro.value.autor = ''
  libro.value.categoria = ''
}

const crearLibro = () => {
  let respuesta = booksStore.crearLibro(
    libro.value.titulo,
    libro.value.autor,
    libro.value.categoria,
  )

  if (respuesta.success) {
    resetLibro()
    alert(respuesta.success)
  } else {
    alert(respuesta.error)
  }
}

const eliminarLibro = (id) => {
  let respuesta = booksStore.eliminarLibro(id)
  if (respuesta.success) {
    alert(respuesta.success)
  } else {
    alert(respuesta.error)
  }
}
</script>

<style scoped lang="css"></style>
