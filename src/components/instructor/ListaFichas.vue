<template>
  <div class="container">
    <div class="row m-2">
      <h1>Centro Teleinformática y Producción Industrial</h1>
    </div>
    <div class="row m-2 justify-content">
      <div class="col-lg-4 pb-2">
        <b-button @click="crearFicha()" variant="primary" size="sm">
          <b-icon icon="plus" class="mr-2"></b-icon>
          <span>Agregar Ficha</span>
        </b-button>
      </div>
    </div>
    <div class="row m-2">
      <div class="table-responsive">
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th scope="col">Identificador Ficha de Caracterización</th>
              <th scope="col">Programa de Formación</th>
              <th scope="col">Proyecto</th>
              <th scope="col">Integrantes</th>
              <th scope="col">Ficha</th>
            </tr>
          </thead>
          <tbody v-for="item in items" :key="item.id">
            <tr>
              <td>{{ item.codigo }}</td>
              <td>{{ item.programa.nombre }}</td>
              <td>
                <b-button @click="fichaProyectos(item.id)" variant="primary" class="m-1">
                  <b-icon icon="eye"></b-icon>
                </b-button>
              </td>
              <td>
                <b-button @click="fichaIntegrantes(item.id)" variant="primary" class="m-1">
                  <b-icon icon="eye"></b-icon>
                </b-button>
              </td>
              <td>
                <b-button @click="detalleEntrega(entrega.id)" variant="primary" class="m-1">
                  <b-icon icon="eye"></b-icon>
                </b-button>
                <b-button @click="editarEntrega(entrega)" variant="warning" class="mr-2">
                  <b-icon icon="pencil"></b-icon>
                </b-button>
                <b-button @click="eliminarEntrega(entrega.id)" variant="danger">
                  <b-icon icon="trash"></b-icon>
                </b-button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

  
  <script>


  export default {
    data() {
      return {
        perfil: this.$store.state.perfil.id,
        fields: [
          { key: 'codigo', label: 'ficha' },
          { key: 'action', label: 'action' } // Agregamos una nueva columna para el botón "Ver"
        ],
        items: null
      };
    },
    methods: {
      async getFichaIntegrantes(id) {
        await this.axios.get("/api/fichas-usuario/" + id + '/').then(response => {
          this.items = response.data
        })
      },
      fichaIntegrantes(id){
          console.log(id)
            this.$router.push('/ficha-integrantes/'+id)
        },
      fichaProyectos(id){
          console.log(id)
            this.$router.push('/ficha-proyectos/'+id)
        },
      crearFicha(){
          
          this.$router.push('/crear-ficha')
        },
      
    },
    mounted() {
        this.getFichaIntegrantes(this.perfil)
    },
  };
  </script>

<style>
.custom-button {
  background-color: #00324D !important;
  color: #FFFFFF;
  border-color: #00324D !important;

}
</style>