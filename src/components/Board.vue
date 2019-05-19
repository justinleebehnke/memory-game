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
let NUM_CARDS = 18

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
        let icons = [
            'fas fa-user-astronaut',
            'fa fa-bug',
            'fa fa-paper-plane',
            'fa fa-paint-brush',
            'fa fa-umbrella',
            'fa fa-bicycle',
            'fa fa-bath',
            'fa fa-university',
            'fa fa-bell',
            'fa fa-book',
            'fa fa-briefcase',
            'fa fa-bullhorn',
            'fa fa-camera-retro',
            'fa fa-coffee',
            'fa fa-envelope',
            'fa fa-bolt',
            'fa fa-gamepad',
            'fa fa-gift',
            'fa fa-graduation-cap',
            'fa fa-truck',
            'fa fa-space-shuttle',
            'fa fa-plane',
            'fa fa-magic',
            'fa fa-hourglass',
            'fa fa-flask',
            'fa fa-flag-checkered',
            'fa fa-car',
            'fa fa-binoculars',
            'fa fa-anchor',
            'fa fa-balance-scale',
            'fa fa-birthday-cake',
            'fa fa-laptop',
            'fa fa-microphone',
            'fa fa-plug',
            'fa fa-puzzle-piece',
            'fa fa-star',
            'fa fa-wrench',
            'fa fa-trophy',
            'fa fa-tree',
            'fa fa-rocket',
            'fa fa-leaf',
            'fa fa-key',
            'fa fa-home',
            'fa fa-user-md'
        ]
        let cards = []
        let value = 0
        for (let i = 0; i < NUM_CARDS; i++) {
            if (i % 2 === 0) {
                value += 1
            }
            cards.push({id: i, value: value, isFaceUp: false, icon: icons[value]})
        }
        this.cards = cards.sort(() => Math.random() - 0.5);
    }
}
</script>

<style scoped>
@media(min-width:150px) {
    .cardContainer {
        grid-template-columns: 1fr 1fr 1fr;
    }
}
@media(min-width:550px) {
    .cardContainer {
        grid-template-columns: 1fr 1fr 1fr 1fr;
    }
}
@media(min-width:800px) {
    .cardContainer {
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
    }
}
.cardContainer {
    display: grid;
    grid-column-gap: 1em;
    grid-row-gap: 1em;
}
.board {
    display: inline-block;
    padding: 0.5em;
}
</style>
