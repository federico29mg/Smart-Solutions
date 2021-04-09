<template>
  <v-container class="fill-height" style="background: #ffffff">
    <v-row justify="center" align="center">
      <v-col cols="10">
        <v-card class="pa-2" outlined>
          <v-card-actions>
            <h1>Edición de Solución</h1>
          </v-card-actions>

          <v-form ref="formSolution" v-model="valid" lazy-validation>
            <v-text-field
              v-model="solution.idIncident"
              label="Código de incidente"
              disabled
              outlined
              color="#43BF6F"
              :rules="[rules.idRequired]"
              required
            ></v-text-field>
            <v-text-field
              v-model="solution.id"
              label="Código de solución"
              disabled
              outlined
              color="#43BF6F"
              :rules="[rules.idRequired]"
              required
            ></v-text-field>
            <v-textarea
              v-model="solution.description"
              color="#43BF6F"
              name="Problema del incidente"
              filled
              outlined
              label="Propuesta solución"
              :rules="[rules.descriptionRequired]"
              auto-grow
            ></v-textarea>
            <v-btn
              :disabled="!valid"
              color="#48C4BF"
              class="mr-4"
              width="100%"
              @click="updateSolution()"
              >Editar Solución</v-btn
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
    let id_solution = params.id;
    return { id_solution };
  },

  data: () => ({
    solution: {},
    valid: true,
    rules: {
      idRequired: (v) => !!v || "El código debe ser ingresado",
      descriptionRequired: (v) => !!v || "La descripcion debe ser ingresada",
    },
  }),
  beforeMount() {
    this.getSolution();
  },

  methods: {
    async getSolution() {
      try {
        //
        let response = await this.$axios.get(
          "http://localhost:3001/solutions/" + this.id_solution
        );
        this.solution = response.data;
      } catch (error) {
        this.$swal
          .fire({
            icon: "error",
            title: "Oops...",
            text: "La solución no existe o hubo un error cargandolo.",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              this.$router.push("/Solutions/" + solution.idIncident);
            }
          });
      }
    },

    async updateSolution() {
      if (this.$refs.formSolution.validate()) {
        let solution = Object.assign({}, this.solution);
        let response = await this.$axios.put(
          "http://localhost:3001/solutions/" + this.id_solution,
          solution
        );
        this.$swal
          .fire({
            icon: "success",
            title: "Operación exitosa.",
            text: "La solución se actualizó correctamente",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              this.$router.push("/Solutions/" + solution.idIncident);
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