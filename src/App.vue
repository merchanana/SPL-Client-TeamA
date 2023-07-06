<template>
  <v-app>
    <v-app-bar app color="#165C66" dark>
      <div class="d-flex justify-space-between align-center header-container">
        <v-img alt="Vuetify Logo" class="shrink mr-2" contain src="./assets/lihtsus-logo.svg"
          transition="scale-transition" width="80" />
        <div>
          <v-btn text to="/" exact>
            Testimonios
          </v-btn>

          <v-btn text to="/crud" exact>
            CRUD
          </v-btn>
        </div>
      </div>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>


<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>

<style>
.header-container {
  width: 100%;
}
</style>
