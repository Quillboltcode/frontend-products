<template>
  <div class="products">
    <h1>Products</h1>
    <div v-if="products.length" class="product-grid">
      <div v-for="product in products" :key="product._id" class="product-card">
        <img v-if="product.image" :src="`http://localhost:3032${product.image}`" :alt="product.name" class="product-image" />
        <div v-else class="product-image-placeholder">No Image</div>
        <div class="product-info">
          <h2 class="product-name">{{ product.name }}</h2>
          <p class="product-price">${{ product.price }}</p>
        </div>
        <button @click="deleteProduct(product._id)" class="delete-btn">Delete</button>
      </div>
    </div>
    <div v-else>
      Loading products...
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const products = ref([]);

const fetchProducts = () => {
  axios.get('http://localhost:3032/api/products')
    .then(response => {
      products.value = response.data.data;
    })
    .catch(error => {
      console.error('Error fetching data:', error);
    });
};

const deleteProduct = (id) => {
  axios.delete(`http://localhost:3032/api/products/${id}`)
    .then(() => {
      // Refresh the product list after deletion
      fetchProducts();
    })
    .catch(error => {
      console.error('Error deleting product:', error);
    });
};

onMounted(fetchProducts);
</script>

<style scoped>
.products {
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

.product-card {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.2s;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.product-card:hover {
  transform: translateY(-5px);
}

.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-image-placeholder {
  width: 100%;
  height: 200px;
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #999;
}

.product-info {
  padding: 15px;
  text-align: center;
}

.delete-btn {
  background-color: orange;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  width: 100%;
}

.delete-btn:hover {
  background-color: darkorange;
}
</style>
