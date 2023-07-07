<template>
  <v-app>
    <v-app-bar app color="#165C66" dark>
      <div class="d-flex justify-space-between align-center header-container">
        <img alt="Lihtsus Logo" src="./assets/lihtsus-logo.svg" width="100" />
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
