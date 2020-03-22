<template>
    <div class="container">
        <b-button 
            v-on:click="startGame()"
            variant="success"
        >
            Draw Cards
        </b-button>
        <div class="row justify-content-center mt-3">
            <PlayingCard 
                v-if="!cardOneFlipped"
                v-on:click.native="flipCard(0)"
                class="mx-3" 
            />
            <PlayingCard 
                v-else
                v-bind:imgsrc=cards[0].image
                class="mx-3" 
            />
            <PlayingCard 
                v-if="!cardTwoFlipped"
                v-on:click.native="flipCard(1)"
                class="mx-3" 
            />
            <PlayingCard 
                v-else
                v-bind:imgsrc=cards[1].image
                class="mx-3" 
            />
            <PlayingCard 
                v-if="!cardThreeFlipped"
                v-on:click.native="flipCard(2)"
                class="mx-3" 
            />
            <PlayingCard 
                v-else
                v-bind:imgsrc=cards[2].image
                class="mx-3" 
            />
            <PlayingCard 
                v-if="!cardFourFlipped"
                v-on:click.native="flipCard(3)"
                class="mx-3" 
            />
            <PlayingCard 
                v-else
                v-bind:imgsrc=cards[3].image
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
        cardOneFlipped: false,
        cardTwoFlipped: false,
        cardThreeFlipped: false,
        cardFourFlipped: false,
        cards: [
            {},
            {},
            {},
            {},
        ],
        gameStarted: false,
    }),
    methods: {
        flipCard(cardNum) {
            switch(cardNum) {
                case 0:
                    this.cardOneFlipped = true
                    break
                case 1:
                    this.cardTwoFlipped = true
                    break
                case 2:
                    this.cardThreeFlipped = true
                    break
                case 3:
                    this.cardFourFlipped = true
                    break
                default:
                    break
            }
        },
        async startGame() {
            this.gameStarted = true
            await axios
                .get('https://deckofcardsapi.com/api/deck/new/draw/?count=4')
                .then(response => {
                    this.cards[0] = response.data.cards[0]
                    this.cards[1] = response.data.cards[1]
                    this.cards[2] = response.data.cards[2]
                    this.cards[3] = response.data.cards[3]
                })
                .catch(error => {
                    console.log(error)
                })
            this.cardOneFlipped = this.cardTwoFlipped = this.cardThreeFlipped = this.cardFourFlipped = false
            console.log(this.cards)
        }
    },
}
</script>