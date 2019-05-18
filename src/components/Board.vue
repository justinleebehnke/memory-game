<template>
    <div class='board'>
        <div class='cardContainer'>
            <Card 
            v-for="card in cards" 
            :key="card.id" 
            :cardValue="card.value"
            :cardIcon="card.icon"
            :isFaceUp="card.isFaceUp"
            :cardID="card.id"
            @clicked="onClickCard"/>
        </div>
    </div>
</template>
<script>
import Card from '../components/Card.vue'
export default {
    components: {
        Card
    },
    data() {
        return {
            cards: [],
            playerScore: 0,
            totalClicks: 0
        }
    },
    methods: {
        onClickCard(cardID) {
            this.totalClicks += 1
            let faceUpCards = []
            this.cards.forEach(card => {
                if (card.isFaceUp) {
                    faceUpCards.push(card)
                }
            })
            if (faceUpCards.length >= this.cards.length-1) {
                this.$confetti.start();
                this.cards.forEach(card => {
                    if (card.id === cardID) {
                        card.isFaceUp = true
                    }
                })
                return
            }
            let valueToCountMap = {}
            faceUpCards.forEach(card => {
                if (valueToCountMap[card.value]) {
                    valueToCountMap[card.value] = valueToCountMap[card.value] + 1
                } else {
                    valueToCountMap[card.value] = 1
                }
            })
            let faceUpSingleValues = []
            for (var key in valueToCountMap) {
                if (valueToCountMap.hasOwnProperty(key)) {
                    if (valueToCountMap[key] == 1) {
                        faceUpSingleValues.push(parseInt(key))
                    }
                }
            }
            let faceUpSingles = []
            faceUpCards.forEach(card => {
                if (card.value === faceUpSingleValues[0] || card.value === faceUpSingleValues[1]) {
                    faceUpSingles.push(card)
                }
            })
            let value = null;
            this.cards.forEach(card => {
                if (card.id == cardID) {
                    value = card.value
                }
            })
            if (faceUpSingles.length === 1) {
                if (faceUpSingles[0].value === value) {
                    this.playerScore += 1
                }
            } else if (faceUpSingles.length === 2) {
                this.cards.forEach(card => {
                    if (card.id === faceUpSingles[0].id || card.id === faceUpSingles[1].id) {
                        card.isFaceUp = !card.isFaceUp
                    }
                })
            }
            this.cards.forEach(card => {
                if (card.id === cardID) {
                    card.isFaceUp = true
                }
            })
        }
    },
    created() {
        let cards = [
                {id: 1, value: 1, isFaceUp: false, icon: 'fas fa-user-astronaut'},
                {id: 2, value: 1, isFaceUp: false, icon: 'fas fa-user-astronaut'},
                {id: 3, value: 2, isFaceUp: false, icon: 'fa fa-bug'},
                {id: 4, value: 2, isFaceUp: false, icon: 'fa fa-bug'},
                {id: 5, value: 3, isFaceUp: false, icon: 'fa fa-paper-plane'},
                {id: 6, value: 3, isFaceUp: false, icon: 'fa fa-paper-plane'},
                {id: 7, value: 4, isFaceUp: false, icon: 'fa fa-paint-brush'},
                {id: 8, value: 4, isFaceUp: false, icon: 'fa fa-paint-brush'},
                {id: 9, value: 5, isFaceUp: false, icon: 'fa fa-umbrella'},
                {id: 10, value: 5, isFaceUp: false, icon: 'fa fa-umbrella'},
                {id: 11, value: 6, isFaceUp: false, icon: 'fa fa-bicycle'},
                {id: 12, value: 6, isFaceUp: false, icon: 'fa fa-bicycle'}
            ]
        this.cards = cards.sort(() => Math.random() - 0.5);
    }
}
</script>

<style scoped>
.board {
    display: inline-block;
    padding: 0.5em;
}
.cardContainer {
    max-width: 35em;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-column-gap: 1em;
    grid-row-gap: 1em;
}
</style>
