<template>
  <b-navbar class="navbar" toggleable="lg">
    <b-navbar-brand href="#">
      <img src="../../assets/logo1.jpg" alt="Logo" class="m-2" style="width: 50px; height: 50px;">
    </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <!-- Enlaces de router a la izquierda -->
        <b-nav-item to="/Inicio">Inicio</b-nav-item>
        <b-nav-item to="/acerca">Acerca</b-nav-item>
      </b-navbar-nav>
      <b-navbar-nav class="ml-auto">
        <!-- Enlaces de router a la derecha -->
        <b-nav-item to="/lista-proyecto">Proyectos</b-nav-item>

        <b-nav-item-dropdown text="Grupos" right>
          <b-dropdown-item to="/crear-grupo">Crear grupo</b-dropdown-item>
          <b-dropdown-item to="/crear-proyecto">Ver Grupo</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item-dropdown text="Mis Proyectos" right>
          <b-dropdown-item to="/crear-proyecto">Crear Proyecto</b-dropdown-item>
          <b-dropdown-item to="/mis-proyectos">Ver Proyecto</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item to="/mis-proyectos">{{ rol }}</b-nav-item>
        <b-nav-item @click="logout()">Cerrar sesión</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

  <script>
  import axios from 'axios'
  import { BNavbar, BNavbarBrand, BNavbarToggle, BCollapse, BNavbarNav, BNavItem, BNavItemDropdown, BDropdownItem } from 'bootstrap-vue';
  
  
  export default {
    data() {
          return {
              rol: this.$store.state.rol,
              
          }
      },
    components: {
      BNavbar,
      BNavbarBrand,
      BNavbarToggle,
      BCollapse,
      BNavbarNav,
      BNavItem,
      BNavItemDropdown,
      BDropdownItem,
    },
    methods: {
          async logout() {
              console.log('logout')
  
              await axios
                .post('api/token/logout/')
                .then(response => {
                  console.log(response)
                  console.log('Logged out')
                })
                .catch(error => {
                  console.log(error)
                })
  
              axios.defaults.headers.common['Authorization'] = ""
  
              localStorage.removeItem('token')
  
              this.$store.commit('removeToken')
              this.$store.commit('clearState')
  
  
              this.$router.push('/inicio')
          }
      }
  }
  </script>
<style>
.navbar {
  padding: 2rem;
  background-color: #39A900 !important;
}

.navbar .nav-link {
  color: #FFFFFF !important;
}
</style>
  
  