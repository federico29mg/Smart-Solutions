<template>
  <v-main>
    <v-container class="fill-height" style="background: #ffffff">
      <v-row justify="center" align="center">
        <v-col cols="5">
          <div style="text-align: center">
            <v-img src="https://i.imgur.com/OeM0r4z.png" />
            <h1 style="text-align: center">El registro ha sido un éxito</h1>
            <br />
            <p>
              Tu registro ha sido recibido y pronto un administrador lo validará
            </p>
            <br />
            <a href="http://localhost:3000"
              >Volver a la página de Inicio de Sesión</a
            >
          </div>
        </v-col>
      </v-row>
      <v-row></v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  layout: "blank-layout",
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