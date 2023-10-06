<template>
  <div class="col-12 col-sm-10 col-md-8 offset-sm-1 offset-md-2">
    <div class="mt-5">
      <form class="border border-primary rounded form-inline" @submit="associate">

        <h2 class="col-12 text-center text-primary mt-3 mb-5">Cree su curso nuevo</h2>

        <div class="form-group col-12">
          <label for="courseName" class="custom-label col-md-3">Nombre de curso</label>
          <input id="courseName" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="text"
            placeholder="Nombre de curso" v-model="courseName" required />
        </div>

        <div class="form-group col-12">
          <label class="custom-label col-md-3 display" for="rol">Duración del curso (en horas)</label>
          <input id="durationHours" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="number"
            placeholder="Ingrese la duración en horas" v-model="durationHours" required />
        </div>
        <div class="col-12 mb-3">
          <button class="col-sm-6 col-md-4 offset-sm-5 offset-md-7 btn btn-primary" type="submit">
            Crear Curso
          </button>
        </div>

      </form>
    </div>
  </div>
</template>
  

  
<script>
import axios from 'axios';

const requestPath = '/profesor/crear-curso';

export default {
  name: "AddCourse",
  data() {
    return {
      courseName: '',
      durationHours: '',
    }
  },
  beforeCreate() {
    axios
      .get(this.$store.state.backURL + requestPath)
      .then(response => {
      });
  },
  methods: {
    associate(event) {
      axios
        .post(this.buildURI(), {
          courseName: this.courseName.trim(),
          durationHours: this.durationHours.trim()
        }
        ).then(response => {
          if (response.status !== 201) {
            alert("Error en la petición. Intente nuevamente");
          } else {
            alert("Se ha creado exitosamente un curso");
          }
        }).catch(response => {
          if (response.response.status === 401) {
            alert("¡Ups! Al parecer tu contraseña es incorrecta o la sesión ha finalizado");
          } else if (response.response.status === 400) {
            alert("¿Estás seguro de que aún no tienes ese rol asignado?");
          } else {
            alert("No es posible conectar con el backend en este momento", response);
          }
        });
      event.preventDefault();
    },
    buildURI() {
      let associatePath = "/profesor/crear-curso";
      return this.$store.state.backURL + associatePath + this.course;
    }
  }

}
</script>
  
<style scoped>
.form-inline .form-group {
  margin-bottom: 1rem;
}
</style>