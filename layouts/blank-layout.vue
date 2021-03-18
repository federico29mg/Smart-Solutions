<template>
  <v-app
    style="
      background: #11998e;
      background: -webkit-linear-gradient(to bottom, #11998e, #38ef7d);
      background: linear-gradient(to bottom, #11998e, #38ef7d);
    "
  >
    <nuxt />

    <v-footer :absolute="!fixed" app>
      <span>Soteria Solutions</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    fixed: false,
  }),
};
</script>

<style>
</style>