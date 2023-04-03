<template>
    
    <user-games :playerId="playerId"> </user-games>
</template>
<script>
const API_URL = "http://localhost:8080/games"


import userGames from "./components/userGames.vue"


export default {
    props: {
        playerId: Number,
        required: true,
    },
    components:{
        
        userGames
        
    },
    data() {
        return {
            
            
            games: []
        }
    },
    created() {
        this.fetchData()
    },
    methods: {
        async fetchData() {
            const url = `${API_URL}`
            this.games = await (await fetch(url)).json()
        },
        removeItem(id) {
            this.games.splice(this.games.map(i => i.id).indexOf(id), 1)
        }
    }
}
</script>