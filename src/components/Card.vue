<template>
    <div class="card" v-on:click="$emit('clicked', cardID)">
        <div class="cardFront" v-if="isFaceUp">
             <i v-bind:class="cardIcon"></i>
        </div>
        <div class="cardBack" v-else/>
    </div>
</template>
<script>
export default {
    props: {
        cardValue: {
            required: true,
            type: Number
        },
        cardIcon: {
            required: true,
            type: String
        },
        isFaceUp: {
            required: true,
            type: Boolean
        },
        cardID: {
            required: true,
            type: Number
        } 
    }
}
</script>
<style>
.card {
    display: grid;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    border-radius: 100px;
}

.card:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.cardFront {
    background-color: #62BD96;
    border: #62BD96 solid 10px;
    width: 6em;
    height: 8.5em;
    border-radius: 10px;
}

.cardBack {
    width: 6em;
    height: 8.5em;
    background-color: #F39342;
    border: #F15839 solid 10px;
    border-radius: 10px;
}

.cardFront i {
    margin-top: 30px;
    display: inline-block;
    font-size: 5em;
    color: #F1E5C0;
}
</style>
