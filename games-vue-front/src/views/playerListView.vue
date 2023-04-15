<template>
    <player-details :player="player"></player-details>
    <player-list :players="players" @deleted="removeItem"></player-list>
    <player-form @submit="fetchData"></player-form>
</template>
<script>
const API_URL = "http://localhost:8080/players"
import gameDetails from "../components/playerDetails.vue"
import gameList from "../components/playerList.vue"
import gameForm from  "../components/playerForm.vue"
    export default{
        components:{
            playerDetails,
            playerList,
            playerForm,
        },
        data() {
        return {
            
            id:0,
            
            player: {
                firstName:"",
                lastName:""
                
            },
            players: []
        }
    },
    created() {
        this.fetchData()
    },
    methods: {
        async fetchData() {
            const url = `${API_URL}`
            this.players = await (await fetch(url)).json()
        },
        removeItem(id) {
            this.players.splice(this.players.map(i => i.id).indexOf(id), 1)
        }
    }
}
</script>