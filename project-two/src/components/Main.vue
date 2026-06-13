<script setup>
    import {ref, computed, onMounted} from 'vue'

    const productData = ref([])
    const isLoading = ref(true)
    const errorMessage = ref (null)

    const filterCategory = ref('')

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

    const uniqueCategories = computed(() => {
        const categories = productData.value.map(product => product.category)
        return Array.from(new Set(categories))
    })

    const filteredProducts = computed(() => {
        if(!filterCategory.value) {
            return productData.value    // If "All Categories", show everything
        }

        return productData.value.filter(product =>
            product.category === filterCategory.value
        )
    })


    onMounted(() => {
        getapi()
    })

</script>

<template>
    <div class="app-container">
        <h1>Tindahan</h1>

        <div class="filter-container">
            <label for="category-select" class="filter-label">Filter</label>
            <select
                id="category-select"
                v-model="filterCategory"
                class="category-dropdown"
            >
                <option value="">All Categories</option>
                <option
                    v-for="category in uniqueCategories"
                    :key="category"
                    :value="category"
                >
                    {{ category }}
                </option>
            </select>
        </div>

        <div v-if="isLoading" class="loading-state">
            Loading products...
        </div>

        <div v-else-if="errorMessage" class="status-msg error">
            Failed to load products: {{ errorMessage }}
        </div>

        <div v-else class="products-grid">
            <div v-for="product in filteredProducts" :key="product.id" class="products-card">
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
        width: 100%;
        max-width: 1024px;
        margin: 0 auto;
        padding: 20px;
        box-sizing: border-box;
        font-family:Verdana, Geneva, Tahoma, sans-serif;
    }

    .filter-container {
        display: flex;
        align-items: left;
        align-self: center;
        gap: 8px;
        margin-bottom: 30px;
        width: 100%
    }

    .filter-label {
        font-size: 0.9rem;
        color: #555;
        font-weight: bold;
        align-self: center;
    }

    .category-dropdown {
        width: 100%;
        max-width: 320px;
        padding: 10px 14px;
        font-size: 1rem;
        border: 2px solid #ddd;
        border-radius: 8px;
        outline: none;
        background-color: white;
        cursor: pointer;
        text-transform: capitalize;
    }

    .category-dropdown:focus {
        border-color: #2c3e50;
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
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 20px;
        margin-top: 20px;
    }

    .products-card {
        border: 1px solid #eee;
        border-radius: 8px;
        padding: 15px;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        background-color: white;
    }

    .products-card img {
        width: 240px;
        height: 240px;
        object-fit: contain;
        background-color: #f9f9f9;
        border-radius: 4px;
        border-color: antiquewhite;
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