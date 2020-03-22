<template>
    <div class="container-fluid">
        <PlayingCard />
        <PlayingCard />
        <PlayingCard />
        <PlayingCard />
    </div>
</template>

<script>
import PlayingCard from './PlayingCard.vue'

export default {
    components: {
        PlayingCard
    },
    data: () => ({
        gameStarted: false,

    }),
    methods: {
        restartGame() {
        },
        startGame() {
            this.gameStarted = true
        }
    }
}
</script>

<style scoped>
</style>