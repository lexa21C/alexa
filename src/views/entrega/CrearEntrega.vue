<template>
  <div class="container ">
    <b-card class="m-3">
      <h1>Realizar entrega del proyecto {{ proyecto.nombre_proyecto }}</h1>
      <b-form>
        <b-form-group
          id="descripcion_entrega"
          label="Descripción de la Entrega:"
          label-for="descripcion_entrega"
        >
          <b-form-textarea
            id="descripcion_entrega"
            v-model="entrega.descripcion_entrega"
            :rows="5"
            required
          ></b-form-textarea>
        </b-form-group>
        <b-form-group
          id="documento"
          label="Documento :"
          label-for="documento"
        >
          <b-form-file
            id="documento"
            v-model="entrega.documento"
            multiple
            accept=".pdf,.doc,.docx"
          ></b-form-file>
        </b-form-group>
        <b-form-group id="tipo_revision" label="Tipo Revison">
          <div>
            <b-form-select v-model="entrega.tipo_revision">
              <b-form-select-option v-for="option in tipo_de_revicion" :key="option.id" :value="option.id" >
                {{ option.nombre }}
              </b-form-select-option>
            </b-form-select>
          </div>
        </b-form-group>
      </b-form>
      <div class=" position-absolute bottom-0 end-0">
        <b-button class="m-1" type="reset" variant="danger">Cancelar</b-button>
        <b-button  @click="crearEntrega()" class="m-1">Enviar</b-button>
      </div>
      {{ entrega }}
    </b-card>
  </div>
</template>



<script>
  export default {
    data() {
      return {
        
        proyecto_id : this.$route.params.id,
        perfil: this.$store.state.perfil.id,
        grupos:null,
        entrega:{
          calificacion: null,
          descripcion_entrega: null,
          documento:null,
          respuesta_instructor: null,
          instructor: null,
          proyecto: null,
          tipo_revision: null,
          autor: null

        },
        proyecto:null,
        tipo_de_revicion:[]
      }
    },
    methods: {

      async getProyecto(id){
            await this.axios('api/proyecto/'+id+'/').then(response=>{
                this.proyecto = response.data

            })
        },
      async getTipoDeRevision(){
            await this.axios('api/tipo_revision/').then(response=>{
                this.tipo_de_revicion = response.data

            })
        },

      async crearEntrega(){
        this.entrega.proyecto=this.proyecto_id
        this.entrega.autor= this.perfil
        console.log(this.entrega)
        await this.axios.post('api/entrega/', this.entrega)
      

      },
    async verProyecto(id){
        this.$router.push('/detalle-proyecto/'+id)
      },
    },
    async mounted(){
      await this.getTipoDeRevision()
      await this.getProyecto(this.proyecto_id)

        
    }
  }
</script>