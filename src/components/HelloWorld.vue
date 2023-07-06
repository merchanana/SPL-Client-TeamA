<template>
  <v-container>
    <h2>Testimonios Software Project Leader</h2>
    <v-row>
      <v-col v-for="testimonio in testimonios" :key="testimonio.id" cols="12" sm="6" md="5" lg="3">
        <v-card shaped class="mb-4">
          <v-card-title class="headline">
            {{ testimonio.nombre }}
          </v-card-title>
          <v-card-text>
            {{ testimonio.descripcion }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  data() {
    return {
      testimonios: [],
    };
  },

  created() {
    this.fetchTestimonios();
  },

  methods: {
    fetchTestimonios() {
      fetch('http://localhost:3000/api/obtener-testimonios')
        .then(response => response.json())
        .then(data => {
          this.testimonios = data;
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
h2 {
  text-align: center;
  font-size: 35px;
  font-weight: bold;
  color: #165C66;
  margin-bottom: 5%;
}

.headline {
  font-size: 18px;
  font-weight: bold;
}

.v-card {
  height: 300px;
  overflow-y: auto;
}
</style>