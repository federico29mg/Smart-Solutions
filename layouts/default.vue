<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item class="text-decoration-none" :to="items[0].to" router exact >
          <v-list-item-action>
            <v-icon>{{ items[0].icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="items[0].title" />
          </v-list-item-content>
        </v-list-item>

        <v-list-item class="text-decoration-none" :to="items[2].to" router exact>
          <v-list-item-action>
            <v-icon>{{ items[2].icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="items[2].title" />
          </v-list-item-content>
        </v-list-item>

        <v-list-item class="text-decoration-none" :to="items[3].to" router exact>
          <v-list-item-action>
            <v-icon>{{ items[3].icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="items[3].title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <v-list>
          <v-list-item class="text-decoration-none" :to="items[1].to" router exact>
            <v-list-item-action>
              <v-icon>{{ items[1].icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="items[1].title" />
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </template>
    </v-navigation-drawer>

    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
    </v-app-bar>

    <v-main>
      <v-container class="fill-height">
        <v-row justify="center" align="center">
          <v-col cols="4">
            <v-img src="https://i.imgur.com/OeM0r4z.png" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>Soteria Solutions &copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-table",
          title: "Ver Incidentes",
          to: "/incident-sst",
        },
        {
          icon: "mdi-logout-variant",
          title: "Cerrar Sesión",
          to: "/",
        },
        {
          icon: "mdi-human-male-male",
          title: "Admin",
          to: "/admin",
        },
        {
          icon: "mdi-information",
          title: "Crear incidente",
          to: "/incident",
        },
      ],
      title: "Smart Solutions",
    };
  },
};
</script>
