<template>
    <div>
        <h2 class="text-4xl font-bold text-center mt-10">Top Rated Recipes</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 m-6">
            <RecipeCard v-for="recipe in paginatedTopRatedRecipes" :key="recipe.id" :recipe="recipe" />
        </div>

        <!-- Pagination Controls -->
        <div class="flex justify-center items-center mt-8">
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
import { ref, computed, onMounted } from 'vue';

const recipes = ref([]);
const currentPage = ref(1);
const itemsPerPage = 4;

// Fetch recipes from JSON file
const fetchRecipes = async () => {
    try {
        const response = await fetch('/recipes.json'); // Ensure this file is inside `/public`
        recipes.value = await response.json();
    } catch (error) {
        console.error('Error fetching recipes:', error);
    }
};

// Fetch data when component mounts
onMounted(fetchRecipes);

// Filter & sort top-rated recipes (Only 4.8+)
const topRatedRecipes = computed(() => {
    return recipes.value
        .filter(recipe => recipe.rate >= 4.8) // ✅ Filter only 4.8 and above
        .sort((a, b) => b.rate - a.rate); // Sort from highest rating
});

// Compute total pages based on filtered top-rated recipes
const totalPages = computed(() => Math.ceil(topRatedRecipes.value.length / itemsPerPage));

// Apply pagination to filtered top-rated recipes
const paginatedTopRatedRecipes = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage;
    const end = start + itemsPerPage;
    return topRatedRecipes.value.slice(start, end);
});

// Pagination controls
const nextPage = () => { if (currentPage.value < totalPages.value) currentPage.value++; };
const prevPage = () => { if (currentPage.value > 1) currentPage.value--; };
const goToPage = (page) => { currentPage.value = page; };
</script>
