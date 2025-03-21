<template>
    <div class="p-6">
        <div v-if="recipe" class="max-w-4xl mx-auto bg-white rounded-lg shadow-lg overflow-hidden">
            <img :src="recipe.image" :alt="recipe.name" class="w-full h-64 object-cover">
            <div class="p-6">
                <h1 class="text-4xl font-bold mb-4">{{ recipe.name }}</h1>
                <p class="text-gray-700 mb-4">{{ recipe.description }}</p>
                <div class="flex justify-between items-center mb-4">
                    <div>
                        <span class="text-gray-700 font-bold">Price:</span>
                        <span class="text-gray-900">{{ recipe.price }}</span>
                        <span v-if="recipe.offerPrice" class="text-red-500 line-through ml-2">{{ recipe.offerPrice }}</span>
                    </div>
                    <div>
                        <span class="text-gray-700 font-bold">Time:</span>
                        <span class="text-gray-900">{{ recipe.time }}</span>
                    </div>
                </div>
                <div class="flex items-center">
                    <span class="text-yellow-500 mr-2">{{ recipe.rate }}</span>
                    <i class="fas fa-star text-yellow-500"></i>
                </div>
            </div>
        </div>
        <div v-else class="text-center text-gray-500">Loading recipe...</div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const recipe = ref(null);

const fetchRecipe = async () => {
    try {
        const response = await fetch('/recipes.json'); // Assuming recipes.json is in the static folder
        const recipes = await response.json();
        recipe.value = recipes.find((r) => r.id === parseInt(route.params.id));
    } catch (error) {
        console.error('Error fetching recipe:', error);
    }
};

onMounted(() => {
    fetchRecipe();
});
</script>
