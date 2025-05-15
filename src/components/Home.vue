<template>
  <div class="welcome">
    <a class="navbar-brand" href="#">Bienvenidos!</a>
  </div>
  <div class="container">
    <div class="card-deck m-0">
      <div class="row">
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model.trim="criterioDeBusquedaNombre"
            placeholder="Ingresar un nombre y/o apellido..."
          />
        </div>

        <div class="col">
          <input
            type="number"
            class="form-control"
            v-model.trim="criterioDeBusquedaDNI"
            placeholder="Ingresar DNI..."
          />
        </div>
      </div>
    </div>
    <div
      class="mt-2"
      v-if="
        deberiaMostrarAdvertenciaPorCriterio(criterioDeBusquedaDNIString) ||
        deberiaMostrarAdvertenciaPorCriterio(criterioDeBusquedaNombre)
      "
    >
      <div
        v-if="deberiaMostrarAdvertenciaPorCriterio(criterioDeBusquedaNombre)"
        class="alert alert-danger my-1"
      >
        Para buscar por <b>NOMBRE / APELLIDO</b>, ingrese 3 o más caracteres
      </div>
      <div
        v-if="deberiaMostrarAdvertenciaPorCriterio(criterioDeBusquedaDNIString)"
        class="alert alert-danger my-1"
      >
        Para buscar por <b>DNI</b>, ingrese 3 o más caracteres
      </div>
    </div>
    <br />
    <div class="card-deck m-0">
      <div class="row">
        <div class="col" v-for="persona in personasFiltradas">
          <div class="card mb-3">
            <div class="card-body">
              <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
              <p class="card-text">dni {{ persona.dni }}</p>
              <a href="#" class="card-link">{{ persona.correo }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      criterioDeBusquedaNombre: "",
      criterioDeBusquedaDNI: "",
      personas: [
        {
          nombre: "Daniel",
          apellido: "Sánchez",
          dni: 20442873,
          correo: "danielsanchez68@hotmail.com",
        },
        {
          nombre: "Daniel",
          apellido: "Pérez",
          dni: 12341236,
          correo: "danielperez@hotmail.com",
        },
        {
          nombre: "Juan",
          apellido: "Perez",
          dni: 11111111,
          correo: "juanperez@hotmail.com",
        },
        {
          nombre: "Juan",
          apellido: "Rodríguez",
          dni: 12341235,
          correo: "juanro@hotmail.com",
        },
        {
          nombre: "Ana",
          apellido: "Garófalo",
          dni: 12341234,
          correo: "anamariagarofalo95@gmail.com",
        },
        {
          nombre: "Ana",
          apellido: "García",
          dni: 11111112,
          correo: "anamariagarcía@gmail.com",
        },
        {
          nombre: "Pedro",
          apellido: "Sánchez",
          dni: 11111113,
          correo: "pedro.sanchez@gmail.com",
        },
      ],
    };
  },
  computed: {
    criterioDeBusquedaDNIString() {
      return this.criterioDeBusquedaDNI.toString();
    },
    personasFiltradas() {
      let filtrados = this.personas;

      if (this.deberiaFiltrarPorCriterio(this.criterioDeBusquedaNombre)) {
        filtrados = this.filtrarPorNombreYApellido(filtrados);
      }
      if (this.deberiaFiltrarPorCriterio(this.criterioDeBusquedaDNIString)) {
        filtrados = this.filtrarPorDNI(filtrados);
      }
      return filtrados;
    },
  },
  methods: {
    getNombreCompleto(persona) {
      return `${persona.nombre} ${persona.apellido}`;
    },

    filtrarPorNombreYApellido(listaPersonas) {
      return listaPersonas.filter((persona) =>
        this.getNombreCompleto(persona)
          .toLowerCase()
          .includes(this.criterioDeBusquedaNombre.toLowerCase())
      );
    },

    filtrarPorDNI(listaPersonas) {
      return listaPersonas.filter((persona) =>
        persona.dni.toString().includes(this.criterioDeBusquedaDNIString)
      );
    },

    deberiaFiltrarPorCriterio(criterio) {
      return criterio.length >= 3;
    },

    deberiaMostrarAdvertenciaPorCriterio(criterio) {
      return criterio.length != 0 && criterio.length < 3;
    },
  },
};
</script>

<style scoped>
.welcome {
  padding: 12px;
  font-size: x-large;
  font-weight: 800;
  background-color: darkmagenta;
  color: white;
}

.container {
  padding: 16px;
}
</style>
