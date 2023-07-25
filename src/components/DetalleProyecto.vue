<template>
    <div>
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <h1>{{ proyecto.nombre }}</h1>
                    <p><span class="fw-lighter">Estado: </span>{{ proyecto.estado }}</p>
                    <p class="fw-lighter">Descripcion:</p>
                    <p>{{ proyecto.descripcion }}</p>
                    <p class="fw-lighter">Repositorio en GitHub:</p>
                    <b-link :href="proyecto.codigo_fuente">{{ proyecto.codigo_fuente }}</b-link>
                    <p class="fw-lighter">Categorías:</p>
                    <ul>
                      <li v-for="categoria in categoria" :key="categoria.id">{{ categoria}}</li>                          
                    </ul>
                </div>
                 <div class="col-lg-4">
                    <img class="img-fluid" src="../assets/2.jpg" alt="">
                </div>
            </div>
      </div>
    </div>
  </template>
  
<script>
import axios from 'axios'

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
    
    methods:{
        idCategoria(categoria){
            for (let i = 0; i < categoria.length; i++) {
                this.getCategoria(categoria[i])
              console.log(`ID: ${categoria[i]}`);
            }
        },

        getCategoria(categorias_id){
            axios.get('api/categoria/'+categorias_id+'/').then(response=>{
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
