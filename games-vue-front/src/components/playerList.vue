<template>
    <table>
      <tr>
        <th>Name</th>
        <th></th>
      </tr>
      <tr v-for="player in players" :key="player.id">
        <td>{{ player.firstName }}</td>
        <td>
          <button id="show-details-modal" @click="playerDetailId = player.id">Show Details</button>
          <button id="show-delete-modal" @click="playerDeleteId = player.id">Delete</button>
        </td>
      </tr>
    </table>
    <Teleport to="body">
      <!-- use the modal component, pass in the prop -->
      <modal :show="playerDetailId !== 0" @close="playerDetailId = 0">
        <template #header>{{ currentplayer.firstName }}
        </template>
        <template #body>
          <playerDetails :player="currentPlayer"></playerDetails>
        </template>
      </modal>
    </Teleport>
    <Teleport to="body">
      <!-- use the modal component, pass in the prop -->
      <modal :show="playerDeleteId !== 0">
        <template #header>Delete {{ currentPlayer.firstName }}
        </template>
        <template #body>
          Are you sure?
        </template>
        <template #footer>
          <div v-if="!error">
            <button class="modal-default-button button-danger" @click="deletePlayer">
              Yes
            </button>
            <button class="modal-default-button button-safe" @click="playerDeleteId = 0">
              No
            </button>
          </div>
          <div v-else>
            <span class="text-danger">{{ error }}</span>
            <button class="modal-default-button button-safe" @click="playerDeleteId = 0">
              OK
            </button>          
          </div>
        </template>
      </modal>
    </Teleport>
  </template>
  <script>
  import playerDetails from "./playerDetails.vue"
  import Modal from "./modal.vue"
  export default {
    components: {
      Modal,
      PlayerDetails
    },
    props: {
      players: Array,
      required: true
    },
    emits: ["deleted"],
    data() {
      return {
        playerDetailId: 0,
        playerDeleteId: 0,
        currentPlayer: null,
        error: null,
      }
    },
    watch: {
      playerDetailId(newId, oldId) {
        this.currentPlayer = this.players.find((item) => item.id == newId)
      },
      playerDeleteId(newId, oldId) {
        this.currentPlayer = this.players.find((item) => item.id == newId)
        this.error = null
      }
    },
    methods: {
      deletePlayer() {
        const api_url = import.meta.env.VITE_API_URL
        fetch(`${api_url}/games/${this.playerDeleteId}`, {
            method: "delete",
        })      
        .then( async response => {
          const isJson = response.headers.get("content-type")?.includes("application/json")
          const data = isJson && await response.json()
          if (! response.ok){
            const error = (data && data.error) || response.status
            return Promise.reject(error)
          }
          this.$emit("deleted", this.playerDeleteId)
          this.playerDeleteId = 0
  
        })
        .catch(error => {
          console.log("Player Delete error: ", error)
          this.error = error
        })      
      }
    },
  }
  </script>
  <style>
  table,
  th,
  td {
    border: 1px solid black;
  }
  .text-danger {
    color:red;
  }
  .button-danger {
    background-color: red;
  }
  .button-safe {
    background-color: green;
  }
  </style>