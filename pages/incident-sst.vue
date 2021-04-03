<template>
  <v-main>
    <v-container class="fill-height" justify-center>
      <v-row>
        <v-col cols="12">
          <v-card class="pa-4">
            <v-card-actions>
              <h1>Listado de incidentes</h1>
              <v-spacer></v-spacer>
              <v-btn color="#48C4BF" class="text-none" to="/incident/"
                >Crear incidente</v-btn
              >
            </v-card-actions>
            <v-card-text>
              <v-data-table
                :headers="headers"
                :items="incidents"
                :items-per-page="5"
                class="elevation-1"
              >
                <template v-slot:item.actions="{ item }">
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="addSolution(item)"
                      small
                    >
                      mdi-plus
                    </v-icon>
                  </v-hover>
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="editItem(item)"
                      small
                    >
                      mdi-pencil
                    </v-icon>
                  </v-hover>
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="deleteItem(item)"
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
  beforeMount() {
    this.getIncidents();
  },
  data() {
    return {
      headers: [
        {
          text: "Código",
          align: "start",
          value: "id",
        },
        { text: "Titulo", value: "title" },
        { text: "Categoría", value: "category" },
        { text: "Acción", value: "actions" },
      ],
      incidents: [],
    };
  },
  methods: {
    /**
     * Enviar una solicitud (Request) en un método get
     * Para consultar todos los productos
     */
    async getIncidents() {
      try {
        let response = await this.$axios.get("http://localhost:3001/incidents");
        this.incidents = response.data;
      } catch (error) {
        console.error(error);
      }
    },

    addSolution(item) {
      let url = `SST/${item.id}`;
      this.$router.push(url);
    },

    editItem(item) {
      let url = `SST/Update/${item.id}`;
      this.$router.push(url);
    },

    deleteItem(item) {
      this.$swal
        .fire({
          icon: "warning",
          title: "¿Está seguro de eliminar el incidente?",
          text: "Al borrar el incidente no se prodrá recuperar.",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = "http://localhost:3001/incidents/" + item.id;
              await this.$axios.delete(url);

              let response = await this.$axios.get(
                "http://localhost:3001/solutions"
              );

              for (var i = 0; i < response.data.length; i++) {
                if (response.data[i].idIncident == item.id) {
                  let url_s =
                    "http://localhost:3001/solutions/" + response.data[i].id;
                  await this.$axios.delete(url_s);
                }
              }

              this.$swal.fire({
                icon: "success",
                title: "Operación exitosa.",
                text: "El item se eliminó correctamente.",
              });
              this.getIncidents();
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