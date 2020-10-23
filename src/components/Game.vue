<template>
    <div class="container">
        <div class="row justify-content-center">
            <b-button 
                v-on:click="startGame()"
                variant="success"
                class="mr-3"
            >
                Draw Cards
            </b-button>
            <RulesModal class="ml-3" />
            <toggle-button 
                class="justify-content-center ml-4 mt-2"
                @change="helpText = $event.value"
                :color="{checked: '#28a745', unchecked: '#6c757d'}"
                :height="22"
                :margin="5"
                :width="90"
                :labels="{checked: 'Help text on', unchecked: 'Help text off'}"
            /> 
        </div>
        <div 
            class="cards mt-3"
        >
            <b-container>
                <b-row>
                    <b-col>
                        <div 
                            v-if="!cardOneFlipped"
                        >
                            <PlayingCard />
                            <b-button 
                                class="m-2" 
                                variant="light" 
                                @click="firstRound('red')"
                            >
                                Red
                            </b-button>
                            <b-button class="m-2" variant="light" @click="firstRound('black')">Black</b-button>
                        </div>
                        
                        <PlayingCard 
                            v-else
                            v-bind:imgsrc=cards[0].image
                        />
                        
                        <p v-if="helpText">Red or Black</p>
                    </b-col>
                    <b-col>
                        <div 
                            v-if="!cardTwoFlipped"
                        >
                            <PlayingCard />
                            <div
                                v-if="cardOneFlipped && !lost"
                            >
                                <b-button class="m-2" variant="light" @click="secondRound('higher')">Higher</b-button>
                                <b-button class="m-2" variant="light" @click="secondRound('lower')">Lower</b-button>
                                <b-button class="m-2" variant="light" @click="secondRound('same')">Same</b-button>
                            </div>
                            
                        </div>
                        
                        <PlayingCard 
                            v-else
                            v-bind:imgsrc=cards[1].image
                        />
                        <p v-if="helpText">Higher, Lower, or Same</p>
                    </b-col>
                    <b-col>
                        <div 
                            v-if="!cardThreeFlipped"
                        >
                            <PlayingCard />
                            <div
                                v-if="cardOneFlipped && cardTwoFlipped && !lost"
                            >
                                <b-button class="m-2" variant="light" @click="thirdRound('inside')">Inside</b-button>
                                <b-button class="m-2" variant="light" @click="thirdRound('outside')">Outside</b-button>
                                <b-button class="m-2" variant="light" @click="thirdRound('same')">Same</b-button>
                            </div>
                        </div>
                        <PlayingCard 
                            v-else
                            v-bind:imgsrc=cards[2].image
                        />
                        <p v-if="helpText">Inside, Outside, or Same</p>
                    </b-col>
                    <b-col>
                        <div 
                            v-if="!cardFourFlipped"
                        >
                            <PlayingCard />
                            <div
                                v-if="cardOneFlipped && cardTwoFlipped && cardThreeFlipped && !lost"
                            >
                                <b-button class="m-2" variant="light" @click="finalRound('hearts')">Hearts</b-button>
                                <b-button class="m-2" variant="light" @click="finalRound('diamonds')">Diamonds</b-button>
                                <b-button class="m-2" variant="light" @click="finalRound('clubs')">Clubs</b-button>
                                <b-button class="m-2" variant="light" @click="finalRound('spades')">Spades</b-button>
                            </div>
                        </div>
                        <PlayingCard 
                            v-else
                            v-bind:imgsrc=cards[3].image
                        />
                        <p v-if="helpText">Spades, Clubs, Hearts, or Diamonds</p>
                    </b-col>
                </b-row>
                <b-row class="justify-content-center mt-3">
                    <p>Times redrawn: {{score}}</p>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
import PlayingCard from './PlayingCard.vue'
import RulesModal from './RulesModal.vue'
import axios from 'axios'
import VueConfetti from 'vue-confetti'
import Vue from 'vue'

Vue.use(VueConfetti)

export default {
    components: {
        PlayingCard,
        RulesModal,
    },
    created() {
        this.startGame()
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
        helpText: false,
        lost: false,
        score: -1,
        won: false,
    }),
    watch: {
        lost() {
            if (this.lost) {
                this.$bvToast.toast('Start over!', {
                    title: 'You got it wrong!',
                    variant: 'danger',
                    solid: true,
                    toaster: 'b-toaster-bottom-center',
                    autoHideDelay: 3000,
                })
            }
        },
        won() {
            if (this.won) {
                this.$bvToast.toast('You truly are the greatest!', {
                    title: 'Congratulations',
                    variant: 'success',
                    solid: true,
                    toaster: 'b-toaster-bottom-center'
                })
            }
        }
    },
    methods: {
        flipCard(cardNum) {
            switch(cardNum) {
                case 0:
                    this.cardOneFlipped = true
                    break
                case 1:
                    if (this.cardOneFlipped) {
                        this.cardTwoFlipped = true
                    }
                    break
                case 2:
                    if (this.cardOneFlipped && this.cardTwoFlipped) {
                        this.cardThreeFlipped = true
                    }
                    break
                case 3:
                    if (this.cardOneFlipped && this.cardTwoFlipped && this.cardThreeFlipped) {
                        this.cardFourFlipped = true
                    }
                    break
                default:
                    break
            }
        },
        async startGame() {
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
            this.cardOneFlipped = this.cardTwoFlipped = this.cardThreeFlipped = this.cardFourFlipped = this.lost = false
            this.score++
            this.$confetti.stop()
        },
        firstRound(color) {
            this.cardOneFlipped = true;

            if ((color === 'red' && (this.cards[0].suit === 'SPADES' || this.cards[0].suit === 'CLUBS')) ||
                (color === 'black' && (this.cards[0].suit === 'HEARTS' || this.cards[0].suit === 'DIAMONDS'))) {
                    this.lost = true
            }
        },
        secondRound(height) {
            this.cardTwoFlipped = true

            const cardOneHeight = this.convertToNumber(0)
            const cardTwoHeight = this.convertToNumber(1)
            
            if ((height === 'higher' && cardOneHeight >= cardTwoHeight) ||
                (height === 'lower' && cardOneHeight <= cardTwoHeight) ||
                (height === 'same' && cardOneHeight !== cardTwoHeight)) {
                    this.lost = true
            }
            
        },
        thirdRound(location) {
            this.cardThreeFlipped = true

            const cardOneNumber = this.convertToNumber(0)
            const cardTwoNumber = this.convertToNumber(1)
            const cardThreeNumber = this.convertToNumber(2)

            if ((location === 'inside' && ((Math.min(cardOneNumber, cardTwoNumber) >= cardThreeNumber) || (Math.max(cardOneNumber, cardTwoNumber) <= cardThreeNumber))) ||
                (location === 'outside' && (Math.min(cardOneNumber, cardTwoNumber) <= cardThreeNumber) && (Math.max(cardOneNumber, cardTwoNumber) >= cardThreeNumber)) ||
                (location === 'same' && cardOneNumber !== cardThreeNumber && cardTwoNumber !== cardThreeNumber)) {
                    this.lost = true
                }
        },
        finalRound(suit) {
            this.cardFourFlipped = true

            if (suit.toUpperCase() !== this.cards[3].suit) {
                this.lost = true
            } else {
                this.won = true;
                this.$confetti.start()
            }
        },
        convertToNumber(card) {
            switch (this.cards[card].value) {
                case 'ACE':
                    return 14
                case 'KING':
                    return 13
                case 'QUEEN':
                    return 12
                case 'JACK':
                    return 11
                default:
                    return Number(this.cards[card].value)
            }
        }
    },
}
</script>

<style scoped lang="sass">
    .cards 
        flex-wrap: nowrap
        justify-content: space-between
</style>