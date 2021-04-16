<template>
  <div class="countdown">
    <h1 class="title">CV Cards</h1>
    <div class="time-info">
        Time remaining: <span class="time-remaining" ref="time" > {{timeRemaining}} </span> sec
    </div>
  </div>
</template>

<script>
import { bus } from '../main'

export default {
    props: ['canStart'],
    data() {
        return {
            timeRemaining: 50,
            timer: ''
        }
    },
    methods: {
        countDown() {
            this.timer = setInterval(() => {
                this.timeRemaining--;

                if (this.timeRemaining > 20) {
                    this.$refs.time.className = 'time-remaining'
                } else if (this.timeRemaining <= 20 && this.timeRemaining > 10) {
                    this.$refs.time.className = 'time-remaining orange'
                } else if (this.timeRemaining <= 10 && this.timeRemaining > 0) {
                    this.$refs.time.className = 'time-remaining red'
                } else if (this.timeRemaining === 0) {
                    this.gameOver()
                }
            }, 1000)
         },
        gameOver() {
            this.$emit('gameEnd', 'Lost')
        },
    },
    watch: {
        canStart: function (newValue) {
            if (newValue === true) {
                this.timeRemaining = 50
                this.countDown()
                this.$refs.time.className = 'time-remaining'
                bus.$emit('newGame')
            } else if (newValue === false) {
                clearInterval(this.timer)
            }
        },
    },
    mounted() {
        bus.$on('addFiveSec', () => {
            this.timeRemaining += 5
        })
    }
 }
</script>

<style>
    .title{
        color: #fff;
        text-align: center;
        font-size: 3rem;
        margin: 3rem;
    }
    .time-info{
        color: #fff;
        text-align: center;
        margin-bottom: 2rem;
        font-size: 2rem;
    }
    .time-remaining{
        font-size: 2.5rem;
        color: #00ff84;
    }
    .time-remaining.orange{
        color: #f88e2b;
    }
    .time-remaining.red {
        color: #f72d1b;
    }


    @media (max-width: 360px){
        .time-info{
            font-size: 1.5rem;
            max-width: 250px;
            margin: 0 auto;
        }
        .time-remaining{
            font-size: 2rem;
        }
    }

</style>