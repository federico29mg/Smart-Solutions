<template>
  <v-main>
    <v-container class="fill-height" style="background: #ffffff">
      <v-row justify="center" align="center">
        <v-col cols="8">
          <v-card outlined>
            <v-card-title style="font-weight: bold"
              >Reporte de Incidente</v-card-title
            >
            <v-card-text>
              <v-form ref="formIncident" v-model="valid" lazy-validation>
                <v-text-field
                  v-model="incident.title"
                  color="#43BF6F"
                  label="Titulo de Incidente"
                  :rules="[rules.titleRequired]"
                  required
                ></v-text-field>

                <v-textarea
                  v-model="incident.description"
                  color="#43BF6F"
                  name="Descripcion"
                  filled
                  clearable
                  clear-icon="mdi-close-circle"
                  label="Descripcion"
                  :rules="[rules.descriptionRequired]"
                  auto-grow
                ></v-textarea>

                <v-select
                  v-model="incident.category"
                  :items="categories"
                  color="#43BF6F"
                  :rules="[rules.categoryRequired]"
                  label="Categoria"
                  required
                ></v-select>
                <br />
                <v-btn
                  :disabled="!valid"
                  color="#48C4BF"
                  class="mr-4"
                  width="100%"
                  @click="saveIncident()"
                >
                  Guardar
                </v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  layout: "blank-layout",

  data: () => ({
    valid: true,

    incident: {
      title: "",
      description: "",
      category: "",
    },
    rules: {
      titleRequired: (v) => !!v || "El titulo debe ser ingresado",
      descriptionRequired: (v) => !!v || "La descripcion debe ser ingresada",
      categoryRequired: (v) => !!v || "La categoria debe ser seleccionada",
    },
    categories: ["Riesgo Alto", "Riesgo Medio", "Riesgo Bajo"],
  }),

  methods: {
    async saveIncident() {
      if (this.$refs.formIncident.validate()) {
        let incident = Object.assign({}, this.incident);

        let response = await this.$axios.post(
          "http://localhost:3001/incidents",
          incident
        );
        this.$swal.fire({
          type: "success",
          title: "Incidente creado",
          showConfirmButton: false,
          timer: 1500,
        });
        this.$refs.formIncident.reset();
      } else {
        this.$swal.fire({
          type: "error",
          title: "Oops...",
          text: "Algo salió mal!",
        });
        this.$refs.formIncident.reset();
      }
    },
  },
};
</script>