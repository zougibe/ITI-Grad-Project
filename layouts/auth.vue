<template>
    <div>
        <!-- Conditionally render the header -->
        <header v-if="!isAuthPage">
            <NuxtLink to="/auth/login">Login</NuxtLink>
            <NuxtLink to="/auth/signup">Sign Up</NuxtLink>
        </header>
        <main>
            <slot /> <!-- This is where the page content will be rendered -->
        </main>
    </div>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
    name: 'auth',
    setup() {
        const route = useRoute();

        // Check if the current route is either the "Sign Up" or "Login" page
        const isAuthPage = route.path === '/auth/signup' || route.path === '/auth/login';

        return {
            isAuthPage
        };
    }
};
</script>