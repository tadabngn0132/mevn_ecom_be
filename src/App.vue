<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

// Declare the backend API
var backendAPI = "http://localhost:4000/api/product";

// Data
const data = ref(null);

// Methods
const fetchProducts = () => {
  axios
    .get(backendAPI)
    .then((response) => {
      data.value = response.data;
    })
    .catch((err) => {
      console.log("Error loading product list: " + err);
    });
};

const deleteProduct = (id) => {
  const confirmed = confirm("Are you sure to delete this product?");
  if (confirmed) {
    axios
      .delete(backendAPI + "/delete/" + id)
      .then(() => {
        fetchProducts();
      })
      .catch((err) => {
        console.error("Error deleting product:" + err);
      });
  }
};

// Fetch products when component is mounted
onMounted(() => {
  fetchProducts();
});

// Helper methods for stats
const getUniqueCategories = () => {
  if (!data.value) return 0;
  const categories = new Set(
    data.value.map((p) => p.category?.name).filter(Boolean)
  );
  return categories.size;
};

const getTotalValue = () => {
  if (!data.value) return 0;
  return data.value.reduce((sum, p) => sum + (p.price || 0), 0).toFixed(2);
};
</script>

<template>
  <div class="app-container">
    <!-- Header -->
    <header class="header">
      <div class="header-content">
        <h1 class="logo">🛍️ Product Management</h1>
        <p class="subtitle">Manage your products with ease</p>
      </div>
    </header>

    <!-- Main Content -->
    <main class="main-content">
      <div class="container">
        <!-- Stats Cards -->
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-icon">📦</div>
            <div class="stat-info">
              <h3>{{ data?.length || 0 }}</h3>
              <p>Total Products</p>
            </div>
          </div>
          <div class="stat-card">
            <div class="stat-icon">📊</div>
            <div class="stat-info">
              <h3>{{ getUniqueCategories() }}</h3>
              <p>Categories</p>
            </div>
          </div>
          <div class="stat-card">
            <div class="stat-icon">💰</div>
            <div class="stat-info">
              <h3>${{ getTotalValue() }}</h3>
              <p>Total Value</p>
            </div>
          </div>
        </div>

        <!-- Products Table -->
        <div class="table-card">
          <div class="table-header">
            <h2>Products List</h2>
            <div class="table-actions">
              <button class="btn btn-refresh" @click="fetchProducts">
                🔄 Refresh
              </button>
            </div>
          </div>

          <div class="table-wrapper">
            <table class="products-table">
              <thead>
                <tr>
                  <th>Product</th>
                  <th>Price</th>
                  <th>Category</th>
                  <th class="text-center">Actions</th>
                </tr>
              </thead>
              <tbody>
                <tr v-if="!data || data.length === 0">
                  <td colspan="4" class="empty-state">
                    <div class="empty-content">
                      <span class="empty-icon">📭</span>
                      <p>No products found</p>
                    </div>
                  </td>
                </tr>
                <tr
                  v-for="product in data"
                  :key="product._id"
                  class="product-row"
                >
                  <td>
                    <div class="product-info">
                      <img
                        :src="product.image"
                        :alt="product.name"
                        class="product-image"
                        @error="$event.target.src = `/images/${product.image}`"
                      />
                      <span class="product-name">{{ product.name }}</span>
                    </div>
                  </td>
                  <td>
                    <span class="price-tag">${{ product.price }}</span>
                  </td>
                  <td>
                    <span class="category-badge">{{
                      product.category?.name || "N/A"
                    }}</span>
                  </td>
                  <td class="text-center">
                    <button
                      @click="deleteProduct(product._id)"
                      class="btn btn-delete"
                    >
                      🗑️ Delete
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

/* Header */
.header {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 2rem 0;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.header-content {
  width: 100%;
  margin: 0 auto;
  padding: 0 2rem;
  text-align: center;
}

.logo {
  font-size: 2.5rem;
  color: white;
  margin-bottom: 0.5rem;
  font-weight: 700;
}

.subtitle {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1.1rem;
}

/* Main Content */
.main-content {
  padding: 2rem;
  width: 100%;
}

.container {
  width: 100%;
  max-width: 100%;
  padding: 0;
}

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
  padding: 0 1rem;
}

.stat-card {
  background: white;
  border-radius: 16px;
  padding: 1.5rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.stat-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.stat-icon {
  font-size: 3rem;
  line-height: 1;
}

.stat-info h3 {
  font-size: 2rem;
  color: #667eea;
  font-weight: 700;
  margin-bottom: 0.25rem;
}

.stat-info p {
  color: #666;
  font-size: 0.9rem;
}

/* Table Card */
.table-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin: 0 1rem;
}

.table-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.table-header h2 {
  font-size: 1.5rem;
  font-weight: 600;
}

.table-actions {
  display: flex;
  gap: 0.5rem;
}

/* Table */
.table-wrapper {
  overflow-x: auto;
}

.products-table {
  width: 100%;
  border-collapse: collapse;
}

.products-table thead {
  background: #f8f9fa;
}

.products-table th {
  padding: 1rem;
  text-align: left;
  font-weight: 600;
  color: #495057;
  border-bottom: 2px solid #dee2e6;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.products-table td {
  padding: 1rem;
  border-bottom: 1px solid #dee2e6;
}

.product-row {
  transition: background-color 0.2s ease;
}

.product-row:hover {
  background-color: #f8f9fa;
}

/* Product Info */
.product-info {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.product-image {
  width: 60px;
  height: 60px;
  border-radius: 8px;
  object-fit: cover;
  border: 2px solid #e9ecef;
}

.product-name {
  font-weight: 500;
  color: #212529;
}

/* Price Tag */
.price-tag {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  font-weight: 600;
  display: inline-block;
}

/* Category Badge */
.category-badge {
  background: #e7f5ff;
  color: #1971c2;
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 500;
  display: inline-block;
}

/* Buttons */
.btn {
  padding: 0.6rem 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 500;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
}

.btn-refresh {
  background: white;
  color: #667eea;
  border: 2px solid white;
}

.btn-refresh:hover {
  background: rgba(255, 255, 255, 0.9);
  transform: scale(1.05);
}

.btn-delete {
  background: #ff6b6b;
  color: white;
}

.btn-delete:hover {
  background: #ff5252;
  transform: scale(1.05);
}

/* Empty State */
.empty-state {
  padding: 3rem;
  text-align: center;
}

.empty-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.empty-icon {
  font-size: 4rem;
  opacity: 0.5;
}

.empty-content p {
  color: #6c757d;
  font-size: 1.1rem;
}

/* Utilities */
.text-center {
  text-align: center;
}

/* Responsive */
@media (max-width: 768px) {
  .logo {
    font-size: 2rem;
  }

  .main-content {
    padding: 1rem;
  }

  .stats-grid {
    grid-template-columns: 1fr;
    padding: 0 0.5rem;
  }

  .table-card {
    margin: 0 0.5rem;
  }

  .table-header {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
    padding: 1rem;
  }

  .product-info {
    flex-direction: column;
    text-align: center;
  }

  .products-table th,
  .products-table td {
    padding: 0.75rem;
    font-size: 0.85rem;
  }
}
</style>
