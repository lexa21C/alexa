<!-- <template>
    <div>
      <body>
        <Carrusel> </Carrusel>  
        <ul>
          <Informacion> </Informacion>
        </ul>
        <ul>
          <Ideas> </Ideas>
        </ul>
       
       </body>
      <Footer>
      </Footer>
    </div>
  </template>
  <script>
    
    import Footer from '../components/Footer.vue'
    import Carrusel from '../components/Carrusel.vue'
    import Informacion from '../components/Informacion.vue'
    import Ideas from '../components/Ideas.vue'
  
  
    export default {
  
      components: {
         Footer, Carrusel, Informacion, Ideas
      }
  
    }
  </script>
  
  <style>
  .imagenes{
    background-color: aqua;
   
  }
  
  
  
  
  </style>
   -->

   <template>
    <b-container>
      <div>
        <b-form-group
          id="documento"
          label="Documento :"
          label-for="documento"
        >
          <b-form-file
            id="documento"
            v-model="documento"
            
            accept=".xls,.xlsx"
            name="file"
          ></b-form-file>
        </b-form-group>
  
        <button @click="uploadFile">Subir archivo</button>
  
        <div>
          <b-form-select v-model="url_rol"  >
            <b-form-select-option  v-for="rol in rol" :key="rol.id" :value="rol.url" >
            {{ rol.nombre }}
            </b-form-select-option>
          </b-form-select>
          
          {{ url_rol }}
        </div>
      </div>
    </b-container>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        url_rol:null,
        rol:null,
        documento: null
      };
    },
    methods: {
      getRol(){
                axios.get("api/rol/").then(response=>{
                this.rol= response.data
              })
            },
      uploadFile() {
        let formData = new FormData();
        formData.append('file', this.documento);
  
        axios
          .post('api/upload/', formData)
          .then(response => {
            console.log(response.data);
          })
          .catch(error => {
            console.error(error);
          });
      }
    },
    mounted(){
      this.getRol()
    }
  };
  </script>