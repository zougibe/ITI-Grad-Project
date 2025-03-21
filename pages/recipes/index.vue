<template>
    <div>
        <!-- Search and Category Filters -->
        <div class="flex justify-between items-center mb-8 ml-3 mr-4">
            <div class="flex items-center w-full max-w-3xl bg-white px-8 py-2 rounded-full shadow-lg mt-5 mr-5">
                <i class="fas fa-search text-gray-400 ml-4"></i>
                <input type="text" v-model="searchQuery" placeholder="Find your Meal..."
                    class="flex-grow p-4 text-black outline-none" />
                <button class="bg-orange-500 text-white px-8 py-3 rounded-full ml-4">Search</button>
            </div>
            <div class="relative flex items-center mt-4">
                <button @click="toggleFilter"
                    class="flex justify-center items-center bg-white p-6 rounded-4xl shadow-lg">
                    <i class="fas fa-sliders-h text-orange-500 mr-2"></i>
                    <span class="text-gray-700">Filter By</span>
                </button>
                <div v-if="showFilter" class="absolute top-full mt-2 w-48 bg-white rounded-lg shadow-lg z-10">
                    <ul>
                        <li @click="selectCategory('')" class="px-4 py-2 hover:bg-gray-100 cursor-pointer">
                            All Categories
                        </li>
                        <li v-for="category in categories" :key="category" @click="selectCategory(category)"
                            class="px-4 py-2 hover:bg-gray-100 cursor-pointer">
                            {{ category }}
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- Loading Spinner -->
        <div v-if="loading" class="flex justify-center items-center h-64">
            <div class="loader"></div>
        </div>

        <!-- Recipe Cards Grid -->
        <div v-else class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5 m-5">
            <RecipeCard v-for="recipe in paginatedRecipes" :key="recipe.id" :recipe="recipe" />
        </div>

        <!-- Pagination Controls -->
        <div v-if="!loading" class="flex justify-center items-center mt-8">
            <button @click="prevPage" :disabled="currentPage === 1"
                class="px-4 py-2 bg-gray-200 rounded-full mr-2 disabled:opacity-50">
                &laquo;
            </button>
            <span v-for="page in totalPages" :key="page" @click="goToPage(page)"
                :class="['px-4 py-2 mx-1 rounded-full cursor-pointer', currentPage === page ? 'bg-orange-500 text-white' : 'bg-gray-200']">
                {{ page }}
            </span>
            <button @click="nextPage" :disabled="currentPage === totalPages"
                class="px-4 py-2 bg-gray-200 rounded-full ml-2 disabled:opacity-50">
                &raquo;
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import RecipeCard from '~/components/RecipeCard.vue';

const recipes = ref([]);
const categories = ref([]); // Dynamically fetched categories
const showFilter = ref(false);
const currentPage = ref(1);
const itemsPerPage = 12;
const searchQuery = ref('');
const selectedCategory = ref('');
const loading = ref(true); // Loading state

// Toggle filter dropdown
const toggleFilter = () => {
    showFilter.value = !showFilter.value;
};

// Select category and reset pagination
const selectCategory = (category) => {
    selectedCategory.value = category;
    showFilter.value = false;
    currentPage.value = 1;
};

// Fetch recipes from `recipes.json`
const fetchRecipes = async () => {
    try {
        loading.value = true; // Start loading
        const response = await fetch('/recipes.json'); // Ensure it's inside `public/`
        const data = await response.json();
        recipes.value = data || [];

        // Extract unique categories dynamically
        const categorySet = new Set(data.map(recipe => recipe.category));
        categories.value = [...categorySet]; // Convert Set to Array
    } catch (error) {
        console.error('Error loading recipes:', error);
        recipes.value = [];
    } finally {
        loading.value = false; // Stop loading
    }
};

// Fetch data on component mount
onMounted(fetchRecipes);

// Filter recipes based on category & search query
const filteredRecipes = computed(() => {
    return (recipes.value || []).filter((recipe) => {
        const matchesCategory =
            selectedCategory.value === '' ||
            recipe.category.toLowerCase() === selectedCategory.value.toLowerCase();
        const matchesSearch = recipe.name.toLowerCase().includes(searchQuery.value.toLowerCase());
        return matchesCategory && matchesSearch;
    });
});

// Pagination logic
const totalPages = computed(() => Math.ceil(filteredRecipes.value.length / itemsPerPage));

const paginatedRecipes = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage;
    const end = start + itemsPerPage;
    return filteredRecipes.value.slice(start, end);
});

// Pagination controls
const prevPage = () => { if (currentPage.value > 1) currentPage.value--; };
const nextPage = () => { if (currentPage.value < totalPages.value) currentPage.value++; };
const goToPage = (page) => { currentPage.value = page; };
</script>

<style>
/* Add a simple loader animation */
.loader {
    border: 4px solid #f3f3f3;
    border-top: 4px solid #FF6B00;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
</style>
