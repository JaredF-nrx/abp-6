<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark py-3">
    <div class="container-fluid justify-content-center">
      <div class="navbar-nav">
        <RouterLink to="/" class="nav-link mx-3 text-light">Home</RouterLink>
        <RouterLink to="/books" class="nav-link mx-3 text-light">Libros</RouterLink>
        <RouterLink to="/profile" class="nav-link mx-3 text-light" v-if="loginStore.sesion"
          >Mi perfil</RouterLink
        >
        <RouterLink to="/login" class="nav-link mx-3 text-light" v-if="!loginStore.sesion"
          >Login</RouterLink
        >
        <a
          href="#"
          @click.prevent="logout"
          class="nav-link mx-3 text-light"
          v-if="loginStore.sesion"
          >Logout</a
        >
      </div>
    </div>
  </nav>

  <RouterView />
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { useRouter } from 'vue-router'
import { useLoginStore } from '@/stores/login.store.js'

const router = useRouter()
const loginStore = useLoginStore()

const logout = () => {
  loginStore.$reset()
  router.push({
    name: 'home',
    params: { message: 'Sesión cerrada correctamente', color: 'success' },
  })
}
</script>

<style>
body {
  background-color: #f8f9fa; /* Gris claro */
}

.navbar-nav .nav-link {
  transition: color 0.3s ease;
}

.navbar-nav .nav-link:hover {
  color: #adb5bd !important; /* gris Bootstrap */
}
</style>
