<template>
    <div>
        <header
            class="bg-cover bg-center bg-[url('/assets/images/background.png')] h-screen flex flex-col items-center justify-center text-white">
            <h1 class="text-6xl font-bold text-center">Best food for your taste</h1>
            <p class="text-lg text-center mt-4">
                Discover delectable cuisine and unforgettable moments<br>in our welcoming, culinary haven.
            </p>
            <div class="mt-10 flex items-center w-full max-w-3xl bg-white px-8 py-2 rounded-full shadow-lg relative">
                <i class="fas fa-search text-gray-400 ml-4"></i>
                <input type="text" v-model="searchQuery" placeholder="Find your Meal.."
                    class="flex-grow p-4 text-black outline-none" @input="handleSearch" />
                <button class="bg-orange-500 text-white px-8 py-3 rounded-full ml-4">Search</button>

                <!-- Dropdown List -->
                <div v-if="filteredRecipes.length > 0"
                    class="absolute top-full left-0 w-full bg-white rounded-lg shadow-lg mt-2 z-10">
                    <ul>
                        <li v-for="recipe in filteredRecipes" :key="recipe.id"
                            class="flex items-center px-4 py-2 hover:bg-gray-100 cursor-pointer"
                            @click="goToRecipe(recipe.id)">
                            <img :src="recipe.image" :alt="recipe.name"
                                class="w-10 h-10 rounded-full mr-4 object-cover" />
                            <span class="text-gray-700">{{ recipe.name }}</span>
                        </li>
                    </ul>
                </div>
            </div>
        </header>

        <!-- Categories Card Component -->
        <categoriesCard />

        <!-- Top Rated Recipes Section -->
        <section class="m-10">
            <router-link to="/recipes/top-rated">
                <h2 class="text-4xl font-bold mt-10 m-4 cursor-pointer hover:text-orange-500">
                    Top Rated Recipes
                </h2>
            </router-link>

            <!-- Loading Spinner -->
            <div v-if="loading" class="flex justify-center items-center h-64">
                <div class="loader"></div>
            </div>

            <!-- Recipe Cards -->
            <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mt-6">
                <RecipeCard v-for="recipe in topRatedRecipes" :key="recipe.id" :recipe="recipe" />
            </div>
        </section>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import { useRouter } from 'vue-router';
import RecipeCard from '~/components/RecipeCard.vue';
import categoriesCard from '~/components/CategoriesCard.vue';

const recipes = ref([]);
const searchQuery = ref('');
const filteredRecipes = ref([]);
const loading = ref(true); // Loading state
const router = useRouter();

onMounted(async () => {
    try {
        loading.value = true; // Start loading
        const response = await fetch('/recipes.json'); // Ensure the file is inside the public folder
        recipes.value = await response.json();
    } catch (error) {
        console.error('Error loading recipes:', error);
    } finally {
        loading.value = false; // Stop loading
    }
});

// Handle search input
const handleSearch = () => {
    if (searchQuery.value.trim() === '') {
        filteredRecipes.value = [];
        return;
    }

    filteredRecipes.value = recipes.value.filter((recipe) =>
        recipe.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
};

// Navigate to recipe details page
const goToRecipe = (id) => {
    router.push(`/recipes/${id}`);
};

// Compute the top-rated recipes
const topRatedRecipes = computed(() => {
    return recipes.value
        .slice() // Create a copy to avoid mutating the original array
        .sort((a, b) => b.rate - a.rate) // Sort by highest rating
        .slice(0, 4); // Get the top 4 recipes
});
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
