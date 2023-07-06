<template>
  <v-container>
    <h1>CRUD Testimonios</h1>
    <div class="btn-container">
      <v-btn color="primary" @click="openCrearModal">Agregar Testimonio</v-btn>
    </div>

    <v-alert v-if="mensaje" type="success" dismissible @input="mensaje = ''">
      {{ mensaje }}
    </v-alert>

    <v-data-table :headers="headers" :items="testimonios" :items-per-page="5" class="elevation-1">
      <!-- eslint-disable-next-line vue/valid-v-slot -->
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="openActualizarModal(item)">mdi-pencil</v-icon>
        <v-icon small @click="eliminarTestimonio(item.id)">mdi-delete</v-icon>
      </template>
    </v-data-table>

    <!-- Modal para agregar testimonio -->
    <v-dialog v-model="crearModal" max-width="500px">
      <v-card>
        <v-card-title class="d-flex justify-space-between align-center">
          <span class="headline">Agregar Testimonio</span>
          <v-btn icon @click="cerrarCrearModal">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-form @submit.prevent="crearTestimonio">
            <v-text-field v-model="nuevoNombre" label="Nombre"></v-text-field>
            <v-textarea v-model="nuevaDescripcion" label="Descripción"></v-textarea>
            <v-btn color="primary" type="submit">Agregar</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>

    <!-- Modal para actualizar testimonio -->
    <v-dialog v-model="actualizarModal" max-width="500px">
      <v-card>
        <v-card-title class="d-flex justify-space-between align-center">
          <span class="headline">Actualizar Testimonio</span>
          <v-btn icon @click="cerrarActualizarModal">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-form @submit.prevent="actualizarTestimonio">
            <v-text-field v-model="testimonioActual.nombre" label="Nombre"></v-text-field>
            <v-textarea v-model="testimonioActual.descripcion" label="Descripción"></v-textarea>
            <v-btn color="primary" type="submit">Actualizar</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      testimonios: [],
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Autor', value: 'nombre' },
        { text: 'Testimonio', value: 'descripcion' },
        { text: 'Acciones', value: 'actions', sortable: false }
      ],
      nuevoNombre: '',
      nuevaDescripcion: '',
      testimonioActual: {},
      mensaje: '',
      crearModal: false,
      actualizarModal: false
    };
  },
  created() {
    // Realizar solicitud HTTP para obtener los testimonios desde el servidor
    this.obtenerTestimonios();
  },
  methods: {
    // Solicitud HTTP GET para obtener los testimonios desde el servidor
    async obtenerTestimonios() {
      try {
        const response = await fetch('http://localhost:3000/api/obtener-testimonios');
        const testimonios = await response.json();
        this.testimonios = testimonios;
      } catch (error) {
        console.error(error);
      }
    },
    // Solicitud HTTP POST para crear un testimonio desde el servidor
    async crearTestimonio() {
      try {
        const testimonio = {
          nombre: this.nuevoNombre,
          descripcion: this.nuevaDescripcion
        };

        const response = await fetch('http://localhost:3000/api/crear-testimonio', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(testimonio)
        });

        if (response.ok) {
          this.nuevoNombre = '';
          this.nuevaDescripcion = '';
          this.mensaje = 'Testimonio agregado correctamente';
          this.obtenerTestimonios();
          this.cerrarCrearModal();
        } else {
          console.error('Error al agregar el testimonio');
        }
      } catch (error) {
        console.error(error);
      }
    },
    // Solicitud HTTP DELETE para eliminar un testimonio desde el servidor
    async eliminarTestimonio(id) {
      try {
        const response = await fetch(`http://localhost:3000/api/eliminar-testimonio/${id}`, {
          method: 'DELETE'
        });

        if (response.ok) {
          this.mensaje = 'Testimonio eliminado correctamente';
          this.obtenerTestimonios();
        } else {
          console.error('Error al eliminar el testimonio');
        }
      } catch (error) {
        console.error(error);
      }
    },
    // Solicitud HTTP PUT para actualizar un testimonio desde el servidor
    async actualizarTestimonio() {
      try {
        const testimonio = {
          id: this.testimonioActual.id,
          nombre: this.testimonioActual.nombre,
          descripcion: this.testimonioActual.descripcion
        };

        const response = await fetch(`http://localhost:3000/api/actualizar-testimonio/${testimonio.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(testimonio)
        });

        if (response.ok) {
          this.mensaje = 'Testimonio actualizado correctamente';
          this.obtenerTestimonios();
          this.cerrarActualizarModal();
        } else {
          console.error('Error al actualizar el testimonio');
        }
      } catch (error) {
        console.error(error);
      }
    },
    openCrearModal() {
      this.crearModal = true;
    },
    openActualizarModal(testimonio) {
      this.testimonioActual = { ...testimonio };
      this.actualizarModal = true;
    },
    cerrarCrearModal() {
      this.crearModal = false;
      this.nuevoNombre = '';
      this.nuevaDescripcion = '';
    },
    cerrarActualizarModal() {
      this.actualizarModal = false;
    },
  },
  watch: {
    mensaje(newValue) {
      if (newValue) {
        setTimeout(() => {
          this.mensaje = '';
        }, 3000);
      }
    }
  }
};
</script>

<style>
h1 {
  font-weight: bold;
  color: #165C66;
}

.btn-container {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 5%;
}

.headline {
  font-size: 20px;
  font-weight: bold;
}
</style>
