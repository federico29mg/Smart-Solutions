<template>
  <v-main>
    <v-container class="fill-height" justify-center>
      <v-row justify="center" align="center">
        <v-col cols="5">
          <v-card class="pa-4">
            <v-card-title class="mb-3" style="font-size: 40px;">
              Soluciones al incidente #{{ id_incident }}
            </v-card-title>
            <v-card-subtitle style="font-size: 20px;">
              {{title}}
            </v-card-subtitle>
            <v-card-text>
              <v-data-table
                :headers="headers"
                :items="solutions"
                :items-per-page="10"
                class="elevation-1"
              >
                <template v-slot:item.actions="{ item }">
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="editSolution(item)"
                      small
                    >
                      mdi-pencil
                    </v-icon>
                  </v-hover>
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="deleteSolution(item)"
                      small
                    >
                      mdi-delete
                    </v-icon>
                  </v-hover>
                </template>
              </v-data-table>
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

  async asyncData({ params }) {
    let id_incident = params.id;
    return { id_incident };
  },

  beforeMount() {
    this.getSolutions();
    this.getIncidentTitle();
  },

  data() {
    return {
      headers: [
        {
          text: "ID Incidente",
          align: "start",
          width: "42%",
          value: "idIncident",
        },
        {
          text: "ID Solución",
          width: "42%",
          value: "id",
        },
        { text: "Acciones", width: "16%", value: "actions" },
      ],
      solutions: [],
      incident: {},

      title: "",
    };
  },
  methods: {
    async getSolutions() {
      try {
        let response1 = await this.$axios.get(
          "http://localhost:3001/solutions"
        );

        for (var i = 0; i < response1.data.length; i++) {
          if (response1.data[i].idIncident == this.id_incident) {
            this.solutions.push(response1.data[i]);
          }
        }
      } catch (error) {
        console.error(error);
      }
    },

    async getIncidentTitle() {
      try {
        let response = await this.$axios.get(
          "http://localhost:3001/incidents"
        );

        for (var i = 0; i < response.data.length; i++) {
          if (response.data[i].id == this.id_incident) {
            this.title = response.data[i].title;
          }
        }
      } catch (error) {
        console.error(error);
      }
    },

    editSolution(item) {
      let url = `Update/${item.id}`;
      this.$router.push(url);
    },

    deleteSolution(item) {
      this.$swal
        .fire({
          icon: "warning",
          title: "¿Está seguro de eliminar la solución?",
          text: "Al borrar la solución, esta no se prodrá recuperar.",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = "http://localhost:3001/solutions/" + item.id;
              await this.$axios.delete(url);

              this.$swal.fire({
                icon: "success",
                title: "Operación exitosa.",
                text: "La solución se eliminó correctamente.",
              });
              this.solutions = [];
              this.getSolutions();
            } catch (error) {
              this.$swal.fire({
                icon: "error",
                title: "Ha ocurrido un problema al eliminar",
                text: error.toString(),
              });
            }
          }
        });
    },
  },
};
</script>

<style>
</style>