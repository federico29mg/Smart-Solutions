<template>
  <v-main>
    <v-container class="fill-height" justify-center>
      <v-row>
        <v-col cols="12">
          <v-card class="pa-4">
            <v-card-actions>
              <h1>Listado de usuarios</h1>
            </v-card-actions>
            <v-card-text>
              <v-data-table
                :headers="headers"
                :items="workers"
                :items-per-page="10"
                class="elevation-1"
              >
                <template v-slot:item.actions="{ item }">
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="editWorker(item)"
                      small
                    >
                      mdi-pencil
                    </v-icon>
                  </v-hover>
                  <v-hover v-slot="{ hover }">
                    <v-icon
                      :color="hover ? '#43BF6F' : 'grey lighten-1'"
                      class="mr-2"
                      @click="deleteWorker(item)"
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
    this.getWorkers();
  },
  data() {
    return {
      headers: [
        {
          text: "Código",
          align: "start",
          value: "id",
        },
        { text: "Cédula", value: "cc" },
        { text: "Nombre", value: "firstName" },
        { text: "Apellido", value: "secondName" },
        { text: "Rol", value: "rol" },
        { text: "Estado", value: "active" },
        { text: "Acciones", width: "5%", value: "actions" },
      ],
      workers: [],
    };
  },
  methods: {
    async getWorkers() {
      try {
        let response = await this.$axios.get("http://localhost:3001/workers");
        this.workers = response.data;
        this.workers.splice(0, 1);
      } catch (error) {
        console.error(error);
      }
    },

    editWorker(item) {
      let url = `Workers/${item.id}`;
      this.$router.push(url);
    },

    deleteWorker(item) {
      this.$swal
        .fire({
          icon: "warning",
          title: "¿Está seguro de eliminar al trabajador?",
          text: "Al borrar el trabajador, este no se prodrá recuperar.",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = "http://localhost:3001/workers/" + item.id;
              await this.$axios.delete(url);

              this.$swal.fire({
                icon: "success",
                title: "Operación exitosa.",
                text: "El trabajador se eliminó correctamente.",
              });
              this.getWorkers();
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