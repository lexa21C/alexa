<template>
    <div class="container">
        <div class="m-2" >
            <b-card>
                <div class="row">
                    <div class="col-lg-1 col-md-1">
                        <img class="imagen" src="@/assets/2.jpg" alt="">
                    </div>
                    <div class="col-lg-9">
                        <h1>{{ proyecto.nombre }}</h1>
                        <p><span class="fw-lighter">Estado: </span>{{ proyecto.estado }}</p>
                    </div>
                </div>        
                <div class="row">
                    <p class="fw-lighter">Descripcion:</p>
                    <p>{{ proyecto.descripcion }}</p>
                </div>
                <div class="row">
                    <p class="fw-lighter">Repositorio en GitHub:</p>
                    <b-link :href="proyecto.codigo_fuente">{{ proyecto.codigo_fuente }}</b-link>
                    <a></a>
                </div>
            
                <div class="row">
                    <b-form>
                        <b-form-group
                        label="Calificacion"
                        >
                            <div class="form-check form-switch" v-for="option in options" :key="option">
                              <input
                                class="form-check-input"
                                type="checkbox"
                                :id="option"
                                :checked="selectedOption === option"
                                @change="selectOption(option)"
                              >
                              <label :for="option">{{ option }}</label>
                            </div>
                        </b-form-group>
                    </b-form>

            </div>
            {{ selectedOption }}
            </b-card>

        </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'

  export default {

    data() {
      return {
        proyecto_estado:null,
        selectedOption: null,
        options: ['en revision','en desarrollo', 'terminado'],
        proyecto: {
                id:null,
                nombre:null,
                codigo_fuente:null,
                categoria:null,
                descripcion:null,
                estado: null,
            },
      };
    },
    methods: {
      selectOption(optionId) {
        if (this.selectedOption === optionId) {
          this.selectedOption = null; // Deseleccionar la opción actual si se hace clic nuevamente en ella
        } else {
          this.selectedOption = optionId; // Establecer la opción seleccionada
          this.editarProyecto()
          this.verProyecto()
        }
      },
      async editarProyecto(){
            let id = this.$route.params.id
            await axios.put("http://127.0.0.1:8000/api/calificar-proyecto/"+id+"/"+this.selectedOption+"/",)
            this.$root.$emit("proyecto-actualizado", id); // Emitir el evento personalizado
           
        },
        async verProyecto (){
        let id = this.$route.params.id
        await axios.get("http://127.0.0.1:8000/api/proyecto/"+id+"/").then(response=>{
            this.proyecto.id=response.data.id
            this.proyecto.nombre=response.data.nombre_proyecto
            this.proyecto.descripcion=response.data.descripcion
            this.proyecto.estado=response.data.estado
            this.proyecto.codigo_fuente=response.data.codigo_fuente

            });
            
            this.selectedOption=this.proyecto.estado   
            // await this.verProyecto()
        },
        

    },
    async mounted(){
        await this.verProyecto()
        
    }
  };
  </script>
  
  
