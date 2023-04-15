<form @submit.prevent="addPlayer">
    <label>
        Name:
        <input type="text" v-model="player.firstName">
    </label>
    <label>
        Last name:
        <input type="text" v-model="player.lastName">
    </label>
    
    <button type="submit">Add Player</button>
</form>
<p>
    <span v-for="error in errors">{{ error }}<br></span>
</p>
</template>
<script>


export default {
data() {
    return {
        player: {
            firstName: "",
            lastName: "",
             
        },
        errors: null
    }
},
computed: {
    canSubmit() {
        return this.firstName.length > 0 && this.lastName.length > 0
    }
},
methods: {
    addPlayer() {
        this.errors = null
        const api_url = import.meta.env.VITE_API_URL
        fetch(api_url + "/players", {
            method: "post",
            body: JSON.stringify(this.player),                
            headers: {'Content-type': 'application/json; charset=UTF-8',
            }
        })
        .then((response) => { return response.json() })
        .then((data) => {                
            if (data.error) {
                this.errors = data.error
                throw data.error                    
            } else {
                this.player = {firstName: "",lastName: ""}
            }
        })
        .catch(error => console.log("Error:",error))
    }
}
}
</script>
<style>
select,
input,
button {
display: block;
margin: 0.5em 0;
font-size: 15px;
}

input[disabled] {
color: #999;
}

p {
color: red;
}
</style>