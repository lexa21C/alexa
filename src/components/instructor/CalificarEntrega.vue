<template>
    <div class="container">
        <b-card class="m-1">
            {{ entrega }}
            <h2>Entrega del  </h2>
            <div class="row">
                <div class="container">
                    <div class="row p-3">
                        <p class="fw-lighter">Descripcion:</p>
                        <p>{{ entrega.descripcion_entrega }}</p>
                    </div>
                    <div class="row">
                        <p><span class="fw-lighter">Tipo De Revision: </span>{{ tipo_revision }}</p>
                    </div>
                    <div class="row ">
                        <b-form>
                            <b-form-group label="Calificacion">
                                <div class="form-check" v-for="option in options" :key="option">
                                  <input
                                    class="form-check-input"
                                    type="radio"
                                    :id="option"
                                    :value="option"
                                    v-model="entrega.calificacion"
                                  >
                                  <label class="form-check-label" :for="option">{{ option }}</label>
                                </div>
                            </b-form-group>
                            <b-form-group
                              id="descripcion_entrega"
                              label="Descripcion de la Calificacion"
                              label-for="descripcion_entrega"
                            >
                              <b-form-textarea
                                id="descripcion_entrega"
                                v-model="entrega.respuesta_instructor"
                                :rows="5"
                                required
                              ></b-form-textarea>
                            </b-form-group>
                        </b-form>
                    </div>
                </div>
            </div>
            <div class="row "  >
                      <div class="ml-auto">
                          <b-button @click="detalleEntrega(entrega.id)" variant="secondary" class="m-1">
                              cancelar
                          </b-button>
                          <b-button @click="calificarEntrega(entrega.id)" variant="success" class="mr-2">
                              Calificar
                          </b-button>
                          
                      </div>
                  </div>
        </b-card>
    </div>
</template>
<script>
import axios from 'axios'
export default{
    data(){
        return{
            perfil: this.$store.state.perfil.id,
            options: ['aprobado','No aprobado'],
            entrega:{
                id:null,
                calificacion: null,
                descripcion_entrega: null,
                documento:null,
                respuesta_instructor: null,
                instructor: null,
                proyecto: null,
                tipo_revision: null,
                autor: null

        },
        tipo_revision:null
        }
    },
    methods:{
       
    
        async getEntrega(){
            let id = this.$route.params.id
            await axios.get('api/entrega/'+id+'/').then(response=>{
                this.entrega.id = response.data.id,
                this.entrega.calificacion= response.data.calificacion,
                this.entrega.descripcion_entrega= response.data.descripcion_entrega,
                // this.entrega.documento= response.data.documento
                this.entrega.respuesta_instructor= response.data.respuesta_instructor,
                this.entrega.instructor= response.data.instructor,
                this.entrega.proyecto= response.data.proyecto,
                this.entrega.tipo_revision= response.data.tipo_revision,
                this.entrega.autor= response.data.autor
            })
            this.entrega.instructor=this.perfil
            

           
        },
        async getTipoDeRevision(id){
            await axios.get('api/tipo_revision/'+id+'/').then(response=>{
                this.tipo_revision=response.data.nombre
            })
        },
        async calificarEntrega(id){
            await axios.put("api/entrega/"+id+"/",this.entrega)
            this.verProyecto(this.entrega.proyecto)
            
           
        },
        async verProyecto(id){
            this.$router.push('/detalle-proyecto/'+id)
      },
    },
    async mounted(){
        await this.getEntrega()
        await this.getTipoDeRevision(this.entrega.tipo_revision)
    }

}
</script>