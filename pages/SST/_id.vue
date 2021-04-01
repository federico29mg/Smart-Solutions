<template>
  <v-container class="fill-height" style="background: #ffffff">
    <v-row justify="center" align="center">
      <v-col cols="10">
        <v-card class="pa-2" outlined>
          <v-card-actions>
            <h1>Respuesta a incidente</h1>
            <v-spacer></v-spacer>
            <v-btn class="text-none" to="/incident-sst"
              >Ver lista de incidentes</v-btn
            >
          </v-card-actions>

          <v-form ref="formSolution" v-model="valid" lazy-validation>
            <v-text-field
              v-model="incident.id"
              label="Código de incidente"
              disabled
              color="#43BF6F"
              :rules="[rules.idRequired]"
              required
            ></v-text-field>
            <v-text-field
              v-model="incident.title"
              :rules="[rules.titleRequired]"
              disabled
              color="#43BF6F"
              label="Titulo de incidente"
              required
            ></v-text-field>
            <v-select
              v-model="incident.category"
              :items="categories"
              color="#43BF6F"
              disabled
              :rules="[rules.categoryRequired]"
              label="Categoria"
              required
            ></v-select>
            <v-textarea
              v-model="incident.description"
              color="#43BF6F"
              name="Problema del incidente"
              filled
              readonly
              label="Problema"
              :rules="[rules.descriptionRequired]"
              auto-grow
            ></v-textarea>
            <v-textarea
              v-model="solution.description"
              color="#43BF6F"
              name="Descripcion de la solución"
              filled
              clearable
              clear-icon="mdi-close-circle"
              label="Descripcion de la solución"
              :rules="[rules.descriptionRequired]"
              auto-grow
            ></v-textarea>
            <v-btn
              :disabled="!valid"
              color="#48C4BF"
              class="mr-4"
              width="100%"
              @click="addSolution()"
              >Agregar solucion</v-btn
            >
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: "blank-layout",

  async asyncData({ params }) {
    let id_incident = params.id;
    return { id_incident };
  },

  data: () => ({
    solution: {
      idIncident: "",
      description: "",
    },
    valid: true,
    incident: {},
    rules: {
      idRequired: (v) => !!v || "El código del incidente debe ser ingresado",
      titleRequired: (v) => !!v || "El titulo debe ser ingresado",
      descriptionRequired: (v) => !!v || "La descripcion debe ser ingresada",
      categoryRequired: (v) => !!v || "La categoria debe ser seleccionada",
    },
    categories: ["Riesgo Alto", "Riesgo Medio", "Riesgo Bajo"],
  }),
  beforeMount() {
    this.getIncident();
  },

  methods: {
    /**
     * Enviar una solicitud (Request) en un método get
     * Para obtener un solo producto dado el código
     */
    async getIncident() {
      try {
        //
        let response = await this.$axios.get(
          "http://localhost:3001/incidents/" + this.id_incident
        );
        this.incident = response.data;
      } catch (error) {
        this.$swal
          .fire({
            type: "error",
            title: "Oops...",
            text: "El incidente no existe o hubo un error cargandolo.",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              this.$router.push("/incident-sst");
            }
          });
      }
    },

    /**
     * Enviar una solicitud (Request) en un método update
     * Para modificar el producto
     */
    async addSolution() {
      if (this.$refs.formSolution.validate()) {
        // Crear un nuevo objeto con la info del producto
        let solution = Object.assign({}, this.solution);
        let response = await this.$axios.post(
          "http://localhost:3001/solutions",
          solution
        );
        this.$swal.fire({
          type: "success",
          title: "Operación exitosa.",
          text: "La solución se agregó correctamente.",
        });
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Formulario incompleto.",
          text: "Hay campos que deben ser diligenciados.",
        });
      }
    },
  },
};
</script>

<style></style>