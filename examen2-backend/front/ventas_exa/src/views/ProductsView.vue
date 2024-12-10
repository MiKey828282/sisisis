<template>
  <div class="products-list">
    <h1>Productos</h1>
    <div v-for="product in products" :key="product.id" class="product-card">
      <img :src="product.Image" :alt="product.name" class="product-image" />
      <h2>{{ product.name }}</h2>
      <p>{{ product.description }}</p>
      <p>Precio: {{ product.price }} MXN</p>
      <p v-if="product.discount">Descuento: {{ product.discount }}%</p>
      <p>Stock: {{ product.stock }}</p>
      <button @click="buyProduct(product)">Comprar</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios';

interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  discount: number | null;
  stock: number;
  Image: string;
}

export default defineComponent({
  name: 'ProductsList',
  setup() {
    const products = ref<Product[]>([]);

    const userId = 1;

    const fetchProducts = async () => {
      try {
        const response = await axios.get<Product[]>('http://localhost:8000/api/products');
        products.value = response.data;
      } catch (error) {
        console.error('Error al cargar los productos:', error);
      }
    };

    const buyProduct = async (product: Product) => {
      if (product.stock <= 0) {
        alert('Este producto no está disponible.');
        return;
      }

      const purchaseData = {
        userId,
        productId: product.id,
        quantity: 1,
        totalPrice: product.price,
      };

      try {
        const response = await axios.post('http://localhost:8000/api/purchases', purchaseData);
        alert(`Compra realizada con éxito: ${response.data.message}`);
      } catch (error) {
        console.error('Error al realizar la compra:', error);
        alert('Hubo un error al procesar la compra.');
      }
    };

    onMounted(fetchProducts);

    return {
      products,
      buyProduct,
    };
  },
});
</script>

<style>
.products-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Tres columnas */
  gap: 20px; /* Espacio entre los productos */
  padding: 20px;
  justify-content: center; /* Centrar la cuadrícula */
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
