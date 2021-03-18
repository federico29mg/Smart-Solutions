<template>
  <v-main>
    <v-container class="fill-height" style="background: #ffffff">
      <v-row justify="center" align="center">
        <v-col cols="3">
          <v-img src="https://i.imgur.com/OeM0r4z.png" />
        </v-col>
        <v-col cols="4">
          <v-card class="pa-2" outlined>
            <v-card-title> Bienvenido de vuelta, </v-card-title>

            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field
                  append-icon="mdi-account"
                  :rules="[rules.userRequired]"
                  color="#43BF6F"
                  label="Usuario"
                  required
                  outlined
                ></v-text-field>

                <v-text-field
                  :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
                  :rules="[rules.passwordRequired]"
                  color="#43BF6F"
                  :type="show ? 'text' : 'password'"
                  label="Contraseña"
                  class="input-group--focused"
                  @click:append="show = !show"
                  required
                  outlined
                ></v-text-field>

                <v-btn
                  :disabled="!valid"
                  color="#48C4BF"
                  @click="validate"
                  tile
                  width="100%"
                >
                  Iniciar Sesión
                </v-btn>
              </v-form>
            </v-card-text>

            <v-card-text class="text-center">
              <a href="http://localhost:3000/recover">¿Olvidó su contraseña?</a>

              <br />

              <a href="http://localhost:3000/register"
                >¿No tiene usuario? Regístrese</a
              >
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

  data() {
    return {
      valid: true,
      show: false,
      row: null,
      rules: {
        userRequired: (v) => !!v || "El usuario debe ser ingresado",
        passwordRequired: (v) => !!v || "La contraseña debe ser ingresada",
      },
    };
  },

  methods: {
    
    validate() {
      this.$refs.form.validate();
    },
  },
};
</script>

<style>
a {
  color: "#41BC48";
  text-decoration: none;
}

a:hover {
  color: "#5EB741";
  text-decoration: underline;
}
</style>