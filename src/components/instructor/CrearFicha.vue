<template>
    <div class="container">
        <b-card>

            <b-form @submit="submitForm" v-if="showForm">
                <b-form-group
                  id="codigo"
                  label="Código"
                  label-for="codigo-input"
                  description="Ingrese el código de la ficha"
                >
                  <b-form-input id="codigo-input" v-model="formData.codigo" type="number" required></b-form-input>
                </b-form-group>
            
                <b-form-group
                  id="fecha-inicio"
                  label="Fecha de inicio"
                  label-for="fecha-inicio-input"
                  description="Seleccione la fecha de inicio"
                >
                  <b-form-datepicker id="fecha-inicio-input" v-model="formData.fecha_inicio" required></b-form-datepicker>
                </b-form-group>
            
                <b-form-group
                  id="fecha-finalizacion"
                  label="Fecha de finalización"
                  label-for="fecha-finalizacion-input"
                  description="Seleccione la fecha de finalización"
                >
                  <b-form-datepicker id="fecha-finalizacion-input" v-model="formData.fecha_finalizacion" required></b-form-datepicker>
                </b-form-group>
            
                <b-form-group
                  id="modalidad"
                  label="Modalidad"
                  label-for="modalidad-select"
                  description="Seleccione la modalidad"
                >
                  <b-form-select id="modalidad-select" v-model="formData.modalidad" :options="modalidadOptions" required></b-form-select>
                </b-form-group>
            
                <b-form-group
                  id="programa"
                  label="Programa"
                  label-for="programa-select"
                  description="Seleccione el programa"
                >
                <b-form-select id="modalidad-select" v-model="formData.programa" v-for="item in programaOptions" :key="item.id" required>
                    <b-form-select-option :value="item.url">
                      {{ item.nombre }}
                    </b-form-select-option>
                  </b-form-select>
                </b-form-group>
                <b-button @click="postFicha()" type="submit" variant="primary">Guardar</b-button>
            </b-form>
           
              
           
            <div v-else>
                <p>El formulario ha sido enviado.</p>
                <p>Código: {{ formData.codigo }}</p>
                <p>Fecha de inicio: {{ formData.fecha_inicio }}</p>
                <p>Fecha de finalización: {{ formData.fecha_finalizacion }}</p>
                <p>Modalidad: {{ formData.modalidad }}</p>
                <p>Programa: {{ formData.programa }}</p>
                
            </div>
        </b-card>
    </div>
  </template>
  
  <script>
  import axios from 'axios'
  import moment from 'moment';

  export default {
    data() {
      return {
        
        showForm: true,
        formData: {
          codigo: null,
          fecha_inicio: null,
          fecha_finalizacion: null,
          modalidad: null,
          programa: null,
        },
        Inscrito:{
          perfil:null,
          
        },
        modalidadOptions: ['presencial', 'virtual'],
        programaOptions:null,
      };
    },
    methods: {
      submitForm(event) {
        event.preventDefault();
        
        // Aquí puedes enviar los datos del formulario al servidor o realizar otras operaciones necesarias
        this.showForm = false; // Ocultar el formulario después de enviarlo
      },
      formatDate(date) {
        return moment(date).format(this.dateFormat);
      },
      async postFicha(){
        this.formData.fecha_inicio = moment(this.formData.fecha_inicio).format('YYYY-MM-DD');
        this.formData.fecha_finalizacion = moment(this.formData.fecha_finalizacion).format('YYYY-MM-DD');
        console.log(this.formData)
        axios.post('api/ficha/', this.formData)
      },
      async getProgramas(){
        axios.get('/api/programa/').then(response => {
          this.programaOptions = response.data
        })
      }

    },
    mounted(){
      this.getProgramas()
    }
  };
  </script>
  