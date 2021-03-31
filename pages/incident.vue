<template>
  <v-main>
    <v-container class="fill-height" style="background: #ffffff">
      <v-row justify="center" align="center">
        <v-col cols="8">
          <v-card-title style="font-weight: bold"
            >Reporte de Incidente</v-card-title
          >
          <v-card-text>
            <v-form ref="formIncident" v-model="valid" lazy-validation>
              <v-text-field
                v-model="incident.title"
                label="Titulo de Incidente"
                :rules="[rules.titleRequired]"
                required
              ></v-text-field>

              <v-textarea
                v-model="incident.description"
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
                :rules="[rules.categoryRequired]"
                label="Categoria"
                required
              ></v-select>

              <v-btn
                :disabled="!valid"
                color="success"
                class="mr-4"
                @click="saveIncident()"
              >
                Guardar
              </v-btn>
            </v-form>
          </v-card-text>
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
          location.href = "http://localhost:3000/success";
      } else {
        console.log("not ok");
      }
    },
  },
};
</script>