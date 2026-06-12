<script setup>
    import {ref, onMounted} from 'vue'

    const productData = ref([])
    const isLoading = ref(true)
    const errorMessage = ref (null)

    const api_url ="https://dummyjson.com/products"

    const getapi = () => {
        isLoading.value = true
        errorMessage.value = null
        fetch(api_url)
            .then(response => {
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`)
                }
                return response.json()
            })
            .then(data => {
                productData.value = data.products
                isLoading.value = false
            })
            .catch(error => {
                console.error("Error fetching products:", error)
                errorMessage.value = error.message
                isLoading.value = false
            })
    }


    onMounted(() => {
        getapi()
    })

</script>

<template>
    <div class="app-container">
        <h1>Product Catalog</h1>

        <div v-if="isLoading" class="loading-state">
            Loading products...
        </div>

        <div v-else-if="errorMessage" class="status-msg error">
            Failed to load products: {{ errorMessage }}
        </div>

        <div v-else class="products-grid">
            <div v-for="product in productData" :key="product.id" class="products-card">
                <img :src="product.thumbnail" :alt="product.title" />
                <h3>{{ product.title }}</h3>
                <p class="category">{{ product.category }}</p>
                <p class="price">${{ product.price }}</p>
            </div>
        </div>
    </div>

</template>


<style scoped>
    .app-container {
        max-width: 1200px;
        margin: 0 auto;
        padding: 20px;
        font-family:Verdana, Geneva, Tahoma, sans-serif;
    }

    .loading {
        text-align: center;
        font-size: 1.2rem;
        margin-top: 40px;
    }

    .status-msg {
        text-align: center;
    }

    .error {
        color: #d9534f;
    }

    .products-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
        gap: 20px;
        margin-top: 20px;
    }

    .products-card {
        border: 1px solid #eee;
        border-radius: 8px;
        padding: 15px;
        text-align: center;
        box-shadow: 0 2px 5px rgba(0,0,0,0.5)
    }

    .product-card img {
        width: 80%;
        height: 150px;
        object-fit: contain;
        background-color: #f9f9f9;
        border-radius: 4px;
    }

    .category {
        color: #777;
        font-size: 0.9rem;
        text-transform: uppercase;
    }

    .price {
        font-weight: bold;
        color: #2c3e50;
    }

    @media (min-width: 1024px) {
        .products-grid {
            grid-template-columns: repeat(4, 1fr);
        }
    }

</style>