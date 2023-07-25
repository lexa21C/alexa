<template>
    <div>
      
      <body>
        <div v-if="rol =='aprendiz' ">
            <EntregaAprendiz></EntregaAprendiz>
        </div>
        <div v-if="rol =='instructor' ">
          <CalificarEntrega></CalificarEntrega>
        </div>
       </body>

    </div>
  </template>
<script>

  import CalificarEntrega from '@/components/instructor/CalificarEntrega.vue'

  import EntregaAprendiz from '@/components/aprendiz/DetalleDeEntrega.vue'
 
  
  
    export default {
      data() {
          return {
              rol: this.$store.state.rol,
              
          }
      },
  
      components: {

    CalificarEntrega,
    EntregaAprendiz,

   },
  
}
  
  </script>