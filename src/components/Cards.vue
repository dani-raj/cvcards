<template>
 <div class="container">
    <div :class="card.class" v-for="card in cards" :key="card.number" @click="turnCard(card)">
        <div class="card-back card-face">
            <img src="../assets/card-back-img.png" alt="" class="card-img">
        </div>
        <div class="card-front card-face">
            <img :src="card.frontImg" alt="" class="card-img">
            <p class="card-description">{{card.description}}
                <a href="https://github.com/dani-raj" target="_blank" v-if="card.link === true" class="github-link">here</a>
            </p>
        </div>
    </div>
 </div>
</template>

<script>
import { bus } from '../main'

export default {
    data() {
        return {
            busy: '',
            selectedCard: null,
            matches: [],
            cards: [
                { number: 1, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/coding.png'), description: 'I can also use jQuery and Bootstrap.', class: 'card'},
                { number: 2, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/synth.png'), description: 'I enjoy spending my spare time making Lo-Fi beats.', class: 'card'  },
                { number: 3, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/design.png'), description: 'I also have some understanding of UI design and experience with Adobe products.', class: 'card' },
                { number: 4, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/github.png'), description: `You can find my GitHub page`, link: true, class: 'card' },
                { number: 5, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/typewriter.png'), description: 'Before I decided to shift to Front-end, I was working in advertising as a creative for 6 years.', class: 'card' },
                { number: 6, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/learn.png'), description: 'Everything I know about coding I learned by myself, Youtube videos, Udemy classes and lots of googling.', class: 'card' },
                { number: 7, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/design.png'), description: 'I also have some understanding of UI design and experience with Adobe products.', class: 'card'},
                { number: 8, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/synth.png'), description: 'I enjoy spending my spare time making Lo-Fi beats.', class: 'card'  },
                { number: 9, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/coding.png'), description: 'I can also use jQuery and Bootstrap.', class: 'card' },
                { number: 10, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/github.png'), description: `You can find my GitHub page`, link: true, class: 'card' },
                { number: 11, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/typewriter.png'), description: 'Before I decided to shift to Front-end, I was working in advertising as a creative for 6 years.', class: 'card' },
                { number: 12, backImg: '../assets/card-back-img.png', frontImg: require('../assets/icons/learn.png'), description: 'Everything I know about coding I learned by myself, Youtube videos, Udemy classes and lots of googling.', class: 'card' },
            ]
        }
    },
    methods: {
        turnCard(card) {
            if(this.canTurnCard(card)) {
                 card.class = 'card visible'
                    if (this.selectedCard)
                        this.isMatch(card);
                    else 
                        this.selectedCard = card 
            }
        },
        canTurnCard(card) {
            return !this.busy && !this.matches.includes(card) && card !== this.selectedCard;
        },
        isMatch(card) {
            if (card.frontImg === this.selectedCard.frontImg) {
                this.cardMatch(card, this.selectedCard);
            } else {
                this.cardMisMatch(card, this.selectedCard)
            }
            this.selectedCard = null
        },
        cardMatch(card1, card2) {
            this.matches.push(card1, card2);
            bus.$emit('addFiveSec')

            if (this.matches.length === this.cards.length) {
                this.$emit('gameEnd', 'Won')
            }
        },
        cardMisMatch(card1,card2) {
            this.busy = true
            setTimeout(() => {
                card1.class = 'card'
                card2.class = 'card'
                this.busy = false;
        }, 1000)},
        shuffleCards() {
            this.cards.sort(function(){return 0.5 - Math.random()})
        },
        newGame() {
            this.cards.forEach(card => {card.class = 'card'}),
            this.matches = []
            this.selectedCard = null
            this.shuffleCards()
            this.busy = false
        },  
    },
    mounted() {
        bus.$on('newGame', this.newGame)
    }
}
</script>

<style scoped>
    .container{
        display: grid;
        grid-template-columns: repeat(4, auto);
        grid-gap: 20px;
        margin: 50px;
        justify-content: center;
        perspective: 800px;
    }
    .card{
        height: 300px;
        width: 200px;
        border-radius: 12px;
        position: relative;
    }
    .card:hover{
        cursor: pointer;
    }
    .card-face{
        position: absolute;
        width: 100%;
        height: 100%;
        padding: 1rem;
        display: flex;
        flex-direction: column;
        text-align: center;
        justify-content: center;
        align-items: center;
        overflow: hidden;
        backface-visibility: hidden;
        border-radius: 20px;
        transition: transform .6s ease-in-out;
    }
    .card-front {
        background: #556f90;
        color: #fff;
        border: none;
        outline: none;
        transform: rotateY(180deg)
    }
    .github-link{
        color: #fff;
    }
    .card-back{
        background: #1f4f8a;
    }
    .card.visible .card-back{
        transform: rotateY(-180deg);
    }
    .card.visible .card-front{
        transform: rotateY(0);
    }
    .card-img{
        width: 80px;
    }
    .card-value {
        width: 80px;
    }

    @media (max-width: 900px){
        .container{
            grid-template-columns: repeat(2, auto);
        }
        .card{
            height: 250px;
            width: 150px;
        }
        .card-img{
            width: 60px;
        }
        .card-description{
            font-size: 14px;
        }
    }

    @media (max-width: 360px){
        .container{
            grid-template-columns: repeat(1, auto);
        }
    }


</style>