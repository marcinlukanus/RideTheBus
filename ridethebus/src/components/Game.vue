<template>
    <div class="container">
        <b-button v-on:click="startGame()">
            Cards
        </b-button>
        <div class="row justify-content-center mt-3">
            <PlayingCard class="mx-3" />
            <PlayingCard class="mx-3" />
            <PlayingCard class="mx-3" />
            <PlayingCard class="mx-3" />
        </div>
    </div>
</template>

<script>
import PlayingCard from './PlayingCard.vue'
import axios from 'axios'

export default {
    components: {
        PlayingCard
    },
    data: () => ({
        cards: [],
        gameStarted: false,
    }),
    methods: {
        async startGame() {
            this.gameStarted = true
            await axios
                .get('https://deckofcardsapi.com/api/deck/new/draw/?count=4')
                .then(response => {
                    this.cards = response.data.cards
                })
                .catch(error => {
                    console.log(error)
                })
            console.log(this.cards)
        }
    },
}
</script>