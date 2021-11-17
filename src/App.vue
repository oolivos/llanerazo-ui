<template>
  <main class="flex-shrink-0">
    <div class="container">
      <h1 class="mt-5">Personas</h1>
      <button
        type="button"
        class="btn btn-success"
        @click="crear = true"
        v-if="!crear"
      >
        Crear
      </button>
      <form class="form" v-if="crear">
        <div class="row">
          <div class="col">
            <input
              type="text"
              class="form-control"
              placeholder="Identifación"
              aria-label="Identificacion"
              v-model="persona.identificacion"
            />
          </div>
          <div class="col">
            <input
              type="text"
              class="form-control"
              placeholder="Nombres"
              aria-label="Nombres"
              v-model="persona.nombres"
            />
          </div>
          <div class="col">
            <input
              type="text"
              class="form-control"
              placeholder="Apellidos"
              aria-label="Apellidos"
              v-model="persona.apellidos"
            />
          </div>
          <div class="col">
            <input
              type="email"
              class="form-control"
              placeholder="Email"
              aria-label="Email"
              v-model="persona.email"
            />
          </div>
          <div class="col">
            <input
              type="password"
              class="form-control"
              placeholder="Password"
              aria-label="Password"
              v-model="persona.password"
            />
          </div>
          <div class="col">
            <button type="button" class="btn btn-success" @click="crearPersona">
              Enviar
            </button>
            <button
              type="button"
              class="btn btn-danger ms-1"
              @click="crear = false"
            >
              Cancelar
            </button>
          </div>
        </div>
      </form>
      <table class="table">
        <tr>
          <th>Documento</th>
          <th>Nombres</th>
          <th>Apellidos</th>
          <th>Email</th>
        </tr>
        <tr v-for="usuario in usuarios" :key="usuario._id">
          <td>{{ usuario.identificacion }}</td>
          <td>{{ usuario.nombres }}</td>
          <td>{{ usuario.apellidos }}</td>
          <td>{{ usuario.email }}</td>
        </tr>
      </table>
    </div>
  </main>
</template>
<script>
import Axios from "axios";
export default {
  name: "App",
  data() {
    return {
      usuarios: [],
      crear: false,
      persona: {
        apellidos: "",
        email: "",
        identificacion: null,
        nombres: "",
        password: "",
      },
    };
  },
  methods: {
    getPersonas() {
      Axios.get("http://localhost:4000/usuarios")
        .then(({ data }) => {
          this.usuarios = data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    crearPersona() {
      Axios.post("http://localhost:4000/usuarios/store", this.persona)
        .then(({ data }) => {
          this.getPersonas();
          alert(`Persona Creada, ID: ${data._id}`)
          this.persona =  {
              apellidos: "",
              email: "",
              identificacion: null,
              nombres: "",
              password: "",
          }
        })
        .catch((error) => {
          alert(error);
        });
    },
  },
  created() {
    this.getPersonas();
  },
};
</script>
