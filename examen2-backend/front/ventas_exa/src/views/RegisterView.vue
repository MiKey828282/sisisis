<template>
  <div class="register-form">
    <h1>Registro</h1>
    <form @submit.prevent="register">
      <label for="name">Nombre:</label>
      <input v-model="name" type="text" placeholder="Nombre" required />

      <label for="lastname">Apellido:</label>
      <input v-model="lastname" type="text" placeholder="Apellido" required />

      <label for="phone">Teléfono:</label>
      <input v-model="phone" type="tel" placeholder="Teléfono" required />

      <label for="email">Correo electrónico:</label>
      <input v-model="email" type="email" placeholder="Correo electrónico" required />

      <button type="submit">Registrarse</button>
    </form>

    <!-- Mensaje de error -->
    <div v-if="errorMessage" class="error-message">
      {{ errorMessage }}
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
  data() {
    return {
      name: '',
      lastname: '',
      phone: '',
      email: '',
      errorMessage: '',  // Variable para mostrar mensaje de error
    };
  },
  methods: {
    async register() {
      try {
        // Intentar registrar el usuario
        const response = await axios.post('http://localhost:8000/api/users', {
          name: this.name,
          lastname: this.lastname,
          phone: this.phone,
          email: this.email,
        });

        // Si el registro es exitoso, redirigir al login
        this.$router.push('/login');
      } catch (error) {
        if (error.response && error.response.status === 400) {
          // Si el error es por correo duplicado
          this.errorMessage = 'Este correo electrónico ya está registrado';
        } else {
          this.errorMessage = 'Hubo un error al registrar el usuario. Intenta nuevamente';
        }
      }
    },
  },
};
</script>

<style scoped>
.error-message {
  color: red;
  margin-top: 10px;
}
</style>


<style>
/* Estilos para el formulario de registro */
.register-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  color: black;
  background-color: #ccc;
}
.register-form label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
}
.register-form input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.register-form button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
