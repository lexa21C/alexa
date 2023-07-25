<template>
    <div class="container">
      <div class="row m-1">
        <h1>Ficha:{{ ficha.codigo }} </h1>
      </div>
      <div class="row m-1 justify-content-end">
      <div class="col-lg-4 pb-2">
        <b-button variant="primary" size="sm" @click="openModal">Agregar</b-button>
      </div>
      <FormularioInscrito :fichaId="ficha.id"  />
      <div class="col-lg-8">
      </div>
    </div>
      <div class="row">
        <div class="table-responsive">
          <table class="table table-striped table-hover">
            <thead>
              <tr>
                <th scope="col">nombre</th>
                <th scope="col">Apellido</th>
                <th scope="col">Rol</th>
                <th scope="col">Acciones</th>

              </tr>
            </thead>
            <tbody v-for="item in items" :key="item.id">
              <tr>
                <td>{{ item.perfil.usuario.first_name }}</td>
                <td>{{ item.perfil.usuario.last_name }}</td>
                <td>{{ item.perfil.rol.nombre}} </td>
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
import FormularioInscrito from '@/components/instructor/AgregarIntegrante.vue';
import axios from 'axios'
  export default {
    props: {
      fichaId: {
        type: Number,
        required: true
      }
    },
    components: {
      FormularioInscrito,
  },
    data() {
      return {
        fields: [
          { key: 'perfil.usuario.first_name', label: 'Nombre' },
          { key: 'perfil.usuario.last_name', label: 'Apellido' },
          { key: 'perfil.rol.nombre', label: 'Rol' }
        ],
        items: null,
        ficha:null,
        modalShow: false,
        
      };
    },
    methods:{
      async getFichaIntegrantes(){
        let id = this.$route.params.id
        axios.get('/api/fichas-integrantes/'+id+'/').then(response => {
          this.items = response.data
        })
      },
      async getFicha(){
        let id = this.$route.params.id
        axios.get('/api/ficha/'+id+'/').then(response => {
          this.ficha = response.data
        })
      },
      openModal() {
      this.modalShow = true;
      this.$emit('modalUpdated', true);
    },
      
    },
    async mounted(){
      await this.getFichaIntegrantes()
      await this.getFicha()
    }
   
  }
  </script>
   