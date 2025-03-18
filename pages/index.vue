<template>
    <div>
        <header
            class="bg-cover bg-center bg-[url('/assets/images/background.png')] h-screen flex flex-col items-center justify-center text-white">
            <h1 class="text-6xl font-bold text-center">Best food for your taste</h1>
            <p class="text-lg text-center mt-4">Discover delectable cuisine and unforgettable moments<br>in our
                welcoming, culinary haven.</p>
            <div class="mt-10 flex items-center w-full max-w-3xl bg-white px-8 py-2 rounded-full shadow-lg">
                <i class="fas fa-search text-gray-400 ml-4"></i>
                <input type="text" placeholder="Find your Meal.." class="flex-grow p-4 text-black outline-none" />
                <button class="bg-orange-500 text-white px-8 py-3 rounded-full ml-4">Search</button>
            </div>
        </header>

        <section class="py-12 flex flex-col items-center">
            <h2 class="text-4xl font-bold text-center mb-8">Categories</h2>
            <div class="flex overflow-x-auto space-x-8 px-4">
                <div v-for="category in categories" :key="category.id" class="flex-shrink-0 flex flex-col items-center">
                    <NuxtLink :to="`/recipes/${category.slug}`" class="flex flex-col items-center">
                        <img :src="category.image" :alt="category.name" class="w-24 h-24 rounded-full mb-4 border-2">
                        <h3 class="text-xl font-semibold">{{ category.name }}</h3>
                    </NuxtLink>
                </div>
            </div>
        </section>
    </div>
</template>

<script setup>
definePageMeta({ layout: "default" })

const categories = [
    { id: 1, name: 'Pizza', slug: 'pizza', image: '/assets/images/category1.png' },
    { id: 2, name: 'Asian Recipes', slug: 'asian-recipes', image: '/assets/images/category2.png' },
    { id: 3, name: 'Sandwiches', slug: 'sandwiches', image: '/assets/images/category3.png' },
    { id: 4, name: 'Sushi', slug: 'sushi', image: '/assets/images/category4.png' },
    { id: 5, name: 'Healthy', slug: 'healthy', image: '/assets/images/category5.png' },
    { id: 6, name: 'Desserts', slug: 'desserts', image: '/assets/images/category6.png' },
    { id: 7, name: 'Cakes', slug: 'cakes', image: '/assets/images/category7.png' },
    { id: 8, name: 'Sea Food', slug: 'sea-food', image: '/assets/images/category8.png' },
    { id: 9, name: 'Chicken', slug: 'chicken', image: '/assets/images/category9.png' }

]
</script>