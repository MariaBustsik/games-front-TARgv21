<template>
    <nav>
        <!-- use the router-link component for navigation. -->
        <!-- specify the link by passing the `to` prop. -->
        <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
        <router-link to="/">Go to Home</router-link>
        <!----<router-link to="/userGames">Go Played Games</router-link> -->
        <router-link to="/players">Players</router-link>

    </nav>
    <!-- route outlet -->
    <!-- component matched by the route will render here -->
    <router-view></router-view>
</template>
<script>
import { RouterLink, RouterView } from 'vue-router';
export default {
    components:{
        RouterLink,
        RouterView
    },
    data() {
        return {}
    }
}
</script>
<style>
nav a{
    margin-left: 1rem;
}
</style>