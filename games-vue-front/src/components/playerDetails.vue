<template>
    <div>
        
        <h3>Name: {{ currentPlayer.firstName }}</h3>
        <h3>Last name: {{ currentPlayer.lastName }}</h3>
    </div>
</template>
<script>
const API_URL = "http://localhost:8080/players"
import useDateFormating from "../composables/useDateFormating.js"

export default {
    props: {
        player: Object,
        required: true
    },
    data() {
        return {
            currentPlayer: { firstName: "", lastName: "" }
        }
    },
    mounted() {
        this.fetchData()
    },
    methods: {
        async fetchData() {
            const url = `${API_URL}`
            this.currentPlayer = await (await fetch(url + "/" + this.player.id)).json()
        },
        formatDate(dateString) {
            return useDateFormating(dateString)
        }
    }
}
</script>