<template>
    <game-details :game="game"></game-details>
    <game-list :games="games" @deleted="removeItem"></game-list>
    <game-form @submit="fetchData"></game-form>
</template>
<script>
const API_URL = "http://localhost:8080/games"
import gameDetails from "../components/gameDetails.vue"
import gameList from "../components/gameList.vue"
import gameForm from  "../components/gameForm.vue"
    export default{
        components:{
            gameDetails,
            gameList,
            gameForm,
        },
        data() {
        return {
            
            id:0,
            
            game: {
                name:"",
                description:"",
                price:0
            },
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