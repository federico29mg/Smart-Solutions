<template>
  <v-container class="fill-height" style="background: #ffffff">
    <v-row justify="center" align="center">
      <v-col cols="10">
        <v-card class="pa-2" outlined>
          <v-card-actions>
            <h1>Edición de Incidente</h1>
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
              color="#43BF6F"
              label="Titulo de incidente"
              required
            ></v-text-field>
            <v-select
              v-model="incident.category"
              :items="categories"
              color="#43BF6F"
              :rules="[rules.categoryRequired]"
              label="Categoria"
              required
            ></v-select>
            <v-textarea
              v-model="incident.description"
              color="#43BF6F"
              name="Problema del incidente"
              filled
              label="Problema"
              :rules="[rules.descriptionRequired]"
              auto-grow
            ></v-textarea>
            <v-btn
              :disabled="!valid"
              color="#48C4BF"
              class="mr-4"
              width="100%"
              @click="updateIncident()"
              >Editar Incidente</v-btn
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
    incident: {},
    valid: true,
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
            icon: "error",
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

    async updateIncident() {
      if (this.$refs.formSolution.validate()) {
        let incident = Object.assign({}, this.incident);
        let response = await this.$axios.put(
          "http://localhost:3001/incidents/" + this.id_incident,
          incident
        );
        this.$swal
          .fire({
            icon: "success",
            title: "Operación exitosa.",
            text: "El incidente se actualizó correctamente",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              this.$router.push("/incident-sst");
            }
          });
      } else {
        this.$swal.fire({
          icon: "warning",
          title: "Formulario incompleto.",
          text: "Hay campos que deben ser diligenciados.",
        });
      }
    },
  },
};
</script>

<style></style>