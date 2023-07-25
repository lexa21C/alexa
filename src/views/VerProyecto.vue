<template>
    <div>
      <div class="container">
          <b-card class="m-1 p-3">
            <div class="row">
              <Proyecto></Proyecto>
            </div>
          <div class="row">
            <h4>Entrega Realizadas:</h4>
            <Entrega></Entrega>
          </div>
          <div class="row">
            <Particpantes :id="proyecto.id"></Particpantes>

          </div>
        </b-card>
      </div>
    </div>
  </template>
  
<script>
import axios from 'axios'
import Proyecto from '@/components/DetalleProyecto.vue'
import Entrega from '@/components/ListaDeEntregas.vue'
import Particpantes from '@/components/ProyectoParticipantes'
export default{
    name:'Lista',

    data(){
        return{
            categoria:[],
            proyecto: {
                id:null,
                nombre:null,
                codigo_fuente:null,
                categorias:null,
                descripcion:null,
                estado: null,
            }
        }
    },
    components: {
      Entrega,
      Particpantes,
      Proyecto
    },
    methods:{
        idCategoria(categoria){
            for (let i = 0; i < categoria.length; i++) {
                this.getCategoria(categoria[i])
              console.log(`ID: ${categoria[i]}`);
            }
        },

        getCategoria(categorias_id){
            axios.get('http://127.0.0.1:8000/api/categoria/'+categorias_id+'/').then(response=>{
                let categoria=response.data.nombre
                this.categoria.push(categoria)
            })

         },
        async verProyecto (){
        let id = this.$route.params.id
        await axios.get("http://127.0.0.1:8000/api/proyecto/"+id+"/").then(response=>{
            this.proyecto.id=response.data.id
            this.proyecto.nombre=response.data.nombre_proyecto
            this.proyecto.descripcion=response.data.descripcion
            this.proyecto.estado=response.data.estado
            this.proyecto.codigo_fuente=response.data.codigo_fuente
            this.proyecto.categorias = response.data.categorias
            });
            this.idCategoria(this.proyecto.categorias)
        }
           
    },
    async mounted(){
        await this.verProyecto()
    }
}
</script>
