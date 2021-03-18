<template>
  <v-main >
    <v-container
      class="fill-height"
      style="background: #FFFFFF"
    >
      <v-row justify="center" align="center">
        <v-col cols="4">
          <v-card class="pa-2" outlined>
            <v-card-title> Bienvenido, </v-card-title>

            <v-card-text>
              <v-form ref="formWorker" v-model="valid" lazy-validation>
                <v-text-field
                  v-model="worker.cc"
                  append-icon="mdi-account"
                  :rules="[rules.userRequired]"
                  color="#43BF6F"
                  label="Cédula de Ciudadanía"
                  required
                  outlined
                ></v-text-field>

                <v-row>
                  <v-col>
                    <v-text-field
                      v-model="worker.firstName"
                      :rules="[rules.firstNameRequired]"
                      color="#43BF6F"
                      label="Nombre"
                      required
                      outlined
                    ></v-text-field>
                  </v-col>

                  <v-col>
                    <v-text-field
                      v-model="worker.secondName"
                      :rules="[rules.secondNameRequired]"
                      color="#43BF6F"
                      label="Apellido"
                      required
                      outlined
                    ></v-text-field>
                  </v-col>
                </v-row>

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

                <v-select
                  v-model="worker.rol"
                  :rules="[rules.rolRequired]"
                  :items="roles"
                  label="Tipo de Empleado"
                  color="#43BF6F"
                  outlined
                  required
                ></v-select>

                <v-btn
                  :disabled="!valid"
                  color="#48C4BF"
                  @click="saveWorker"
                  tile
                  width="100%"
                >
                  Registrarse
                </v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </v-col>

        <v-col cols="3">
          <v-img src="https://i.imgur.com/OeM0r4z.png" />
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  layout: "blank-layout",

  data() {
    return {
      worker: {
        cc: "",
        firstName: "",
        secondName: "",
        email: "",
        password: "",
        rol: "",
        active: false,
      },
      valid: true,
      show: false,
      row: null,
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
    };
  },

  methods: {
    async saveWorker() {
      if (this.$refs.formWorker.validate()) {
        let worker = Object.assign({}, this.worker)
        let response = await this.$axios.post("http://localhost:3001/workers", worker)
        location.href = "http://localhost:3000/success"
      } else {
        console.log("not ok");
      }
    },
  },
};
</script>