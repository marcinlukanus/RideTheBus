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
                            <PlayingCard
                                v-on:click.native="flipCard(0)"
                            />
                            <b-button class="m-2" variant="light">Red</b-button>
                            <b-button class="m-2" variant="light">Black</b-button>
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
                            <PlayingCard 
                                v-on:click.native="flipCard(1)"
                            />
                            <b-button class="m-2" variant="light">Higher</b-button>
                            <b-button class="m-2" variant="light">Lower</b-button>
                            <b-button class="m-2" variant="light">Same</b-button>
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
                            <PlayingCard
                                v-on:click.native="flipCard(2)"
                            />
                            <b-button class="m-2" variant="light">Inside</b-button>
                            <b-button class="m-2" variant="light">Outside</b-button>
                            <b-button class="m-2" variant="light">Same</b-button>
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
                            <PlayingCard 
                                v-on:click.native="flipCard(3)"
                            />
                            <b-button class="m-2" variant="light">Hearts</b-button>
                            <b-button class="m-2" variant="light">Diamonds</b-button>
                            <b-button class="m-2" variant="light">Clubs</b-button>
                            <b-button class="m-2" variant="light">Spade</b-button>
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
        score: -1,
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
            this.score++
        }
    },
}
</script>

<style scoped lang="sass">
    .cards 
        flex-wrap: nowrap
        justify-content: space-between
</style>