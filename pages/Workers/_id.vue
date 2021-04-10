<template>
  <v-container class="fill-height" style="background: #ffffff">
    <v-row justify="center" align="center">
      <v-col cols="10">
        <v-card class="pa-2" outlined>
          <v-card-title class="pa-1 pb-5">
            <h1>Trabajador</h1>
          </v-card-title>

          <v-form ref="formWorker" v-model="valid" lazy-validation>
            <v-text-field
              v-model="worker.id"
              label="iD de Trabajador"
              disabled
              color="#43BF6F"
              :rules="[rules.idRequired]"
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="worker.cc"
              :rules="[rules.userRequired]"
              color="#43BF6F"
              label="Cédula de Ciudadanía"
              disabled
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="worker.firstName"
              :rules="[rules.firstNameRequired]"
              color="#43BF6F"
              label="Nombre"
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="worker.secondName"
              :rules="[rules.secondNameRequired]"
              color="#43BF6F"
              label="Apellido"
              required
              outlined
            ></v-text-field>
            <v-text-field
              v-model="worker.email"
              append-icon="mdi-email"
              :rules="rules.emailRequired"
              color="#43BF6F"
              label="Correo Electrónico"
              required
              outlined
            ></v-text-field>

            <v-text-field
              v-model="worker.password"
              :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              :rules="[rules.passwordRequired]"
              color="#43BF6F"
              :type="show ? 'text' : 'password'"
              label="Contraseña"
              class="input-group--focused"
              margin-bottom="0px"
              @click:append="show = !show"
              required
              outlined
            ></v-text-field>
            <v-row>
              <v-col cols="10" >
                <v-select
                  v-model="worker.rol"
                  :rules="[rules.rolRequired]"
                  :items="roles"
                  label="Tipo de Empleado"
                  color="#43BF6F"
                  outlined
                  required
                ></v-select>
              </v-col>
              <v-col>
                <v-checkbox
                  v-model="worker.active"
                  label="Activo"
                  color="#43BF6F"
                  hide-details
                ></v-checkbox>
              </v-col>
            </v-row>
            <v-btn 
              :disabled="!valid"
              color="#48C4BF"
              class="mb-5"
              width="100%"
              @click="updateWorker()"
              >Actualizar Trabajador</v-btn
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
    let id_worker = params.id;
    return { id_worker };
  },

  data: () => ({
    valid: true,
    show: false,
    worker: {
      cc: "",
      firstName: "",
      secondName: "",
      email: "",
      password: "",
      rol: "",
      active: "",
    },
    roles: ["Planta", "SST"],
    rules: {
      userRequired: (v) => !!v || "El usuario debe ser ingresado",
      firstNameRequired: (v) => !!v || "El nombre debe ser ingresado",
      secondNameRequired: (v) => !!v || "El apellido debe ser ingresado",
      passwordRequired: (v) => !!v || "La contraseña debe ser ingresada",
      rolRequired: (v) => !!v || "Debe elegir rol de empleado",
      emailRequired: [
        (v) => !!v || "El correo electrónico debe ser ingresado",
        (v) => /.+@.+\..+/.test(v) || "El correo electrónico debe ser válido",
      ],
    },
  }),
  beforeMount() {
    this.getWorker();
  },

  methods: {
    /**
     * Enviar una solicitud (Request) en un método get
     * Para obtener un solo producto dado el código
     */
    async getWorker() {
      try {
        //
        let response = await this.$axios.get(
          "http://localhost:3001/workers/" + this.id_worker
        );
        this.worker = response.data;
      } catch (error) {
        this.$swal
          .fire({
            icon: "error",
            title: "Oops...",
            text: "El trabajador no existe o hubo un error cargandolo.",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              this.$router.push("/admin");
            }
          });
      }
    },

    async updateWorker() {
      if (this.$refs.formWorker.validate()) {
        let worker = Object.assign({}, this.worker);
        let response = await this.$axios.put(
          "http://localhost:3001/workers/" + this.id_worker,
          worker
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
              this.$router.push("/admin");
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