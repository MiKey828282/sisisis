<template>
  <div class="product-form">
    <form @submit.prevent="submit">
      <label for="name">Nombre del producto:</label>
      <input id="name" v-model="name" type="text" required />

      <label for="description">Descripción:</label>
      <textarea id="description" v-model="description" required></textarea>

      <label for="price">Precio:</label>
      <input id="price" v-model="price" type="number" required />

      <label for="stock">Stock:</label>
      <input id="stock" v-model="stock" type="number" required />

      <label for="discount">Descuento:</label>
      <input id="discount" v-model="discount" type="number" required />

      <label for="image">Imagen:</label>
      <input id="image" type="file" @change="handleFileUpload" required />

      <button type="submit">Guardar</button>
    </form>
  </div>
</template>

<script lang="ts">
import axios from 'axios'

export default {
  name: 'AdminProductForm',
  data() {
  return {
    name: '',
    description: '',
    price: 0,
    stock: 0,
    discount: 0,
    image: null as File | null, // Update the type to File | null
  }
},
  methods: {
    handleFileUpload(event: Event) {
  const inputElement = event.target as HTMLInputElement;
  if (inputElement.files) {
    this.image = inputElement.files[0];
  } else {
    console.error('No files selected');
  }
},
    submit() {
      const formData = new FormData()
      formData.append('name', this.name)
      formData.append('price', this.price.toString())
      formData.append('description', this.description)
      formData.append('stock', this.stock.toString())
      formData.append('discount', this.discount.toString())

      if (this.image) {
        formData.append('Image', this.image)
      }

      axios
        .post('http://localhost:8000/api/products', formData, {
          headers: { 'Content-Type': 'multipart/form-data' },
        })
        .then((response) => {
          console.log('Producto guardado:', response.data)
          this.$emit('refresh') // Notifica al componente padre para que actualice la lista
        })
        .catch((error) => {
          console.error('Error al guardar producto:', error)
        })
    },
  },
}
</script>

<style>
.product-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  color: black;
  background-color: #ccc;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 20px;
}

.form-group {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

label {
  font-weight: bold;
  margin-bottom: 35px;
  color: #333;
}

input[type='text'],
input[type='number'],
textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  color: #333;
  background-color: #fff;
}

input[type='file'] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  color: #333;
  background-color: #fff;
}
</style>
