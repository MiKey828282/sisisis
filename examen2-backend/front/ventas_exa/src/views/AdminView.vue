<template>
  <div class="admin-view">
    <h1>Administración</h1>
    <a href="/history">Ver historial de compras</a>

    <AdminProductForm
      @refresh="fetchProducts"
      :productToEdit="selectedProduct"
      @resetEdit="resetSelectedProduct"
    />
    <ul class="products-grid">
  <li class="product-card" v-for="product in products" :key="product.id">
    <img :src="product.Image" :alt="product.name" class="product-image" />
    <h2>{{ product.name }}</h2>
    <p>{{ product.description }}</p>
    <p>Precio: {{ product.price }} MXN</p>
    <p v-if="product.discount">Descuento: {{ product.discount }}%</p>
    <p>Stock: {{ product.stock }}</p>
    <button @click="editProduct(product)">Editar</button>
    <button @click="deleteProduct(product.id)">Eliminar</button>
  </li>
</ul>


  </div>
</template>

<script lang="ts">
import axios from 'axios';
import AdminProductForm from '../components/AdminProductForm.vue';
import {useRouter} from 'vue-router';

export default {
  data() {
    return {
      products: [],
      selectedProduct: null,
    };
  },
  components: { AdminProductForm },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('http://localhost:8000/api/products');
        this.products = response.data;
      } catch (error) {
        console.error('Error al cargar productos:', error);
      }
    },
    editProduct(product) {
      this.selectedProduct = product;
    },

  deleteProduct(productId) {
      axios.delete(`http://localhost:8000/api/products/${productId}`)
  .then(response => {
    console.log('Producto eliminado:', response.data);
    this.fetchProducts();
    this.resetSelectedProduct(); // Add parentheses here
  })
  .catch(error => {
    console.error('Error al eliminar producto:', error);
  });
},

    resetSelectedProduct() {
      this.selectedProduct = null;
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>

<style scoped>
.history-view {
  max-width: 600px;
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
  height: 100vh;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}
a {
  margin: 10px auto;
  background-color: #50d403;
  text-decoration: none;
  color: rgb(255, 255, 255);
  border-radius: 5px;
  padding: 10px 20px;
  margin-top: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;

}

a:hover {
  background-color: #45a049;
}
.products-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  padding: 20px;
  list-style: none;
  margin: 0;
  justify-content: center;
  
}

.product-card {
  padding: 10px;
  text-align: center;
  background-color: #f2f2f2;
  color: black;
  border-radius: 10px;
  transition: transform 0.3s ease;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.product-card:hover {
  transform: scale(1.05);
}

.product-image {
  max-width: 100%;
  height: auto;
}

h1 {
  text-align: center;
  margin-top: 20px;
  font-size: 40px;
}

</style>
