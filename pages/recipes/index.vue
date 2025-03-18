<template>
    <div>
        <div class="flex justify-between items-center mb-8 ml-2.5 mr-2.5">
            <div class="flex items-center w-full max-w-3xl bg-white px-8 py-2 rounded-full shadow-lg mt-5">
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
                        <li @click="selectCategory('')" class="px-4 py-2 hover:bg-gray-100 cursor-pointer">All
                            Categories</li>
                        <li v-for="category in categories" :key="category.id" @click="selectCategory(category.slug)"
                            class="px-4 py-2 hover:bg-gray-100 cursor-pointer">{{ category.name }}</li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8">
            <div v-for="recipe in filteredRecipes" :key="recipe.id" class="border p-4 rounded-lg shadow-lg">
                <img :src="recipe.image" :alt="recipe.name" class="w-full h-48 object-cover rounded-lg mb-4">
                <h2 class="text-2xl font-semibold mb-2">{{ recipe.name }}</h2>
                <div class="flex justify-between items-center mb-2">
                    <div>
                        <span class="text-gray-700">{{ recipe.price }}</span>
                        <span class="text-red-500 line-through ml-2">{{ recipe.offerPrice }}</span>
                    </div>
                    <div>
                        <span class="text-gray-700">{{ recipe.time }}</span>
                    </div>
                </div>
                <div class="flex items-center">
                    <span class="text-yellow-500 mr-2">{{ recipe.rate }}</span>
                    <i class="fas fa-star text-yellow-500"></i>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const searchQuery = ref('')
const selectedCategory = ref('')
const showFilter = ref(false)

const toggleFilter = () => {
    showFilter.value = !showFilter.value
}

const selectCategory = (category) => {
    selectedCategory.value = category
    showFilter.value = false
}

const categories = [
    { id: 1, name: 'Pizza', slug: 'pizza' },
    { id: 2, name: 'Chicken', slug: 'chicken' },
    { id: 3, name: 'Asian Recipes', slug: 'asian-recipes' },
    { id: 4, name: 'Sandwiches', slug: 'sandwiches' },
    { id: 5, name: 'Sushi', slug: 'sushi' },
    { id: 6, name: 'Healthy', slug: 'healthy' },
    { id: 7, name: 'Desserts', slug: 'desserts' },
    { id: 8, name: 'Cakes', slug: 'cakes' },
    { id: 9, name: 'Seafood', slug: 'sea-food' }
]

const recipes = ref([
    // 🍕 Pizza
    { id: 1, name: 'Margherita Pizza', category: 'pizza', image: '/assets/images/margherita.jpg', price: '$12', offerPrice: '$10', time: '30 mins', rate: '4.8' },
    { id: 2, name: 'Pepperoni Pizza', category: 'pizza', image: '/assets/images/pepperoni.jpg', price: '$15', offerPrice: '$13', time: '35 mins', rate: '4.7' },
    { id: 3, name: 'BBQ Chicken Pizza', category: 'pizza', image: '/assets/images/bbq-chicken.jpg', price: '$16', offerPrice: '$14', time: '40 mins', rate: '4.9' },
    { id: 4, name: 'Veggie Pizza', category: 'pizza', image: '/assets/images/veggie.jpg', price: '$14', offerPrice: '$12', time: '30 mins', rate: '4.5' },
    { id: 5, name: 'Cheese Burst Pizza', category: 'pizza', image: '/assets/images/cheese-burst.jpg', price: '$18', offerPrice: '$15', time: '45 mins', rate: '4.6' },

    // 🍗 Chicken
    { id: 6, name: 'Chicken Curry', category: 'chicken', image: '/assets/images/chicken-curry.jpg', price: '$15', offerPrice: '$12', time: '30 mins', rate: '4.5' },
    { id: 7, name: 'Grilled Chicken', category: 'chicken', image: '/assets/images/grilled-chicken.jpg', price: '$17', offerPrice: '$14', time: '35 mins', rate: '4.8' },
    { id: 8, name: 'Fried Chicken', category: 'chicken', image: '/assets/images/fried-chicken.jpg', price: '$14', offerPrice: '$12', time: '25 mins', rate: '4.7' },
    { id: 9, name: 'Chicken Tikka', category: 'chicken', image: '/assets/images/chicken-tikka.jpg', price: '$16', offerPrice: '$13', time: '30 mins', rate: '4.6' },
    { id: 10, name: 'Butter Chicken', category: 'chicken', image: '/assets/images/butter-chicken.jpg', price: '$18', offerPrice: '$15', time: '40 mins', rate: '4.9' },

    // 🍜 Asian Recipes
    { id: 11, name: 'Sweet & Sour Chicken', category: 'asian-recipes', image: '/assets/images/sweet-sour.jpg', price: '$14', offerPrice: '$12', time: '25 mins', rate: '4.6' },
    { id: 12, name: 'Teriyaki Chicken', category: 'asian-recipes', image: '/assets/images/teriyaki.jpg', price: '$16', offerPrice: '$14', time: '30 mins', rate: '4.8' },
    { id: 13, name: 'Kung Pao Shrimp', category: 'asian-recipes', image: '/assets/images/kung-pao.jpg', price: '$17', offerPrice: '$15', time: '35 mins', rate: '4.7' },
    { id: 14, name: 'Pad Thai', category: 'asian-recipes', image: '/assets/images/pad-thai.jpg', price: '$15', offerPrice: '$13', time: '30 mins', rate: '4.8' },
    { id: 15, name: 'Beef Bulgogi', category: 'asian-recipes', image: '/assets/images/bulgogi.jpg', price: '$18', offerPrice: '$16', time: '40 mins', rate: '4.9' },

    // 🥪 Sandwiches
    { id: 16, name: 'Club Sandwich', category: 'sandwiches', image: '/assets/images/club.jpg', price: '$10', offerPrice: '$8', time: '15 mins', rate: '4.7' },
    { id: 17, name: 'Philly Cheesesteak', category: 'sandwiches', image: '/assets/images/philly.jpg', price: '$12', offerPrice: '$10', time: '20 mins', rate: '4.8' },
    { id: 18, name: 'Grilled Cheese', category: 'sandwiches', image: '/assets/images/grilled-cheese.jpg', price: '$8', offerPrice: '$6', time: '10 mins', rate: '4.6' },
    { id: 19, name: 'BLT Sandwich', category: 'sandwiches', image: '/assets/images/blt.jpg', price: '$9', offerPrice: '$7', time: '15 mins', rate: '4.7' },
    { id: 20, name: 'Chicken Panini', category: 'sandwiches', image: '/assets/images/panini.jpg', price: '$11', offerPrice: '$9', time: '20 mins', rate: '4.8' },

    // 🍣 Sushi
    { id: 21, name: 'California Roll', category: 'sushi', image: '/assets/images/california-roll.jpg', price: '$14', offerPrice: '$12', time: '25 mins', rate: '4.9' },
    { id: 22, name: 'Salmon Nigiri', category: 'sushi', image: '/assets/images/salmon-nigiri.jpg', price: '$16', offerPrice: '$14', time: '20 mins', rate: '4.8' },
    { id: 23, name: 'Spicy Tuna Roll', category: 'sushi', image: '/assets/images/tuna-roll.jpg', price: '$15', offerPrice: '$13', time: '30 mins', rate: '4.7' },
    { id: 24, name: 'Dragon Roll', category: 'sushi', image: '/assets/images/dragon-roll.jpg', price: '$18', offerPrice: '$16', time: '35 mins', rate: '4.9' },
    { id: 25, name: 'Shrimp Tempura Roll', category: 'sushi', image: '/assets/images/tempura-roll.jpg', price: '$17', offerPrice: '$15', time: '30 mins', rate: '4.8' },

    // 🍰 Cakes
    { id: 26, name: 'Chocolate Cake', category: 'cakes', image: '/assets/images/chocolate-cake.jpg', price: '$8', offerPrice: '$6', time: '20 mins', rate: '4.9' },
    { id: 27, name: 'Vanilla Sponge Cake', category: 'cakes', image: '/assets/images/vanilla-sponge.jpg', price: '$9', offerPrice: '$7', time: '25 mins', rate: '4.8' },
    { id: 28, name: 'Red Velvet Cake', category: 'cakes', image: '/assets/images/red-velvet.jpg', price: '$10', offerPrice: '$8', time: '30 mins', rate: '4.7' },
    { id: 29, name: 'Strawberry Shortcake', category: 'cakes', image: '/assets/images/strawberry-shortcake.jpg', price: '$11', offerPrice: '$9', time: '35 mins', rate: '4.8' },
    { id: 30, name: 'Carrot Cake', category: 'cakes', image: '/assets/images/carrot-cake.jpg', price: '$10', offerPrice: '$8', time: '30 mins', rate: '4.9' },

    // 🍤 Seafood
    { id: 31, name: 'Grilled Salmon', category: 'sea-food', image: '/assets/images/grilled-salmon.jpg', price: '$18', offerPrice: '$15', time: '35 mins', rate: '4.9' },
    { id: 32, name: 'Lobster Tail', category: 'sea-food', image: '/assets/images/lobster-tail.jpg', price: '$25', offerPrice: '$22', time: '45 mins', rate: '4.8' },
    { id: 33, name: 'Garlic Shrimp', category: 'sea-food', image: '/assets/images/garlic-shrimp.jpg', price: '$17', offerPrice: '$14', time: '30 mins', rate: '4.7' },
    { id: 34, name: 'Fish Tacos', category: 'sea-food', image: '/assets/images/fish-tacos.jpg', price: '$14', offerPrice: '$12', time: '20 mins', rate: '4.6' },
    { id: 35, name: 'Crab Cakes', category: 'sea-food', image: '/assets/images/crab-cakes.jpg', price: '$16', offerPrice: '$13', time: '30 mins', rate: '4.8' },

    //🍞 Healthy
    { id: 36, name: 'Quinoa Salad', category: 'healthy', image: '/assets/images/quinoa-salad.jpg', price: '$10', offerPrice: '$8', time: '20 mins', rate: '4.7' },
    { id: 37, name: 'Avocado Toast', category: 'healthy', image: '/assets/images/avocado-toast.jpg', price: '$8', offerPrice: '$6', time: '15 mins', rate: '4.6' },
    { id: 38, name: 'Grilled Salmon', category: 'healthy', image: '/assets/images/grilled-salmon.jpg', price: '$18', offerPrice: '$15', time: '35 mins', rate: '4.8' },
    { id: 39, name: 'Fruit Bowl', category: 'healthy', image: '/assets/images/fruit-bowl.jpg', price: '$12', offerPrice: '$10', time: '10 mins', rate: '4.9' },
    { id: 40, name: 'Green Smoothie', category: 'healthy', image: '/assets/images/green-smoothie.jpg', price: '$7', offerPrice: '$5', time: '5 mins', rate: '4.7' },

    //🍩 Desserts
    { id: 41, name: 'Chocolate Cake', category: 'desserts', image: '/assets/images/chocolate-cake.jpg', price: '$8', offerPrice: '$6', time: '20 mins', rate: '4.9' },
    { id: 42, name: 'Apple Pie', category: 'desserts', image: '/assets/images/apple-pie.jpg', price: '$7', offerPrice: '$5', time: '25 mins', rate: '4.8' },
    { id: 43, name: 'Brownies', category: 'desserts', image: '/assets/images/brownies.jpg', price: '$6', offerPrice: '$4', time: '15 mins', rate: '4.7' },
    { id: 44, name: 'Cheesecake', category: 'desserts', image: '/assets/images/cheesecake.jpg', price: '$9', offerPrice: '$7', time: '30 mins', rate: '4.8' },
    { id: 45, name: 'Tiramisu', category: 'desserts', image: '/assets/images/tiramisu.jpg', price: '$10', offerPrice: '$8', time: '35 mins', rate: '4.9' }

])

const filteredRecipes = computed(() => {
    return recipes.value.filter(recipe => {
        const matchesCategory = selectedCategory.value === '' || recipe.category === selectedCategory.value
        const matchesSearch = recipe.name.toLowerCase().includes(searchQuery.value.toLowerCase())
        return matchesCategory && matchesSearch
    })
})
</script>
