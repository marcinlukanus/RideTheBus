<template>
    <div class="container">
        <b-button v-on:click="startGame()">
            Cards
        </b-button>
        <div class="row justify-content-center mt-3">
            <PlayingCard 
                v-bind:imgsrc=cards.card1.image
                class="mx-3" 
            />
            <PlayingCard 
                v-bind:imgsrc=cards.card2.image
                class="mx-3" 
            />
            <PlayingCard 
                v-bind:imgsrc=cards.card3.image
                class="mx-3" 
            />
            <PlayingCard 
                v-bind:imgsrc=cards.card4.image
                class="mx-3" 
            />
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
        cards: {
            card1: {},
            card2: {},
            card3: {},
            card4: {},
        },
        gameStarted: false,
    }),
    methods: {
        async startGame() {
            this.gameStarted = true
            await axios
                .get('https://deckofcardsapi.com/api/deck/new/draw/?count=4')
                .then(response => {
                    this.cards.card1 = response.data.cards[0]
                    this.cards.card2 = response.data.cards[1]
                    this.cards.card3 = response.data.cards[2]
                    this.cards.card4 = response.data.cards[3]
                })
                .catch(error => {
                    console.log(error)
                })
            console.log(this.cards)
        }
    },
}
</script>