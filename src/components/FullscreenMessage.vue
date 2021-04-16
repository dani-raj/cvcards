<template>
    <div class="fullscreenMessages" @click="newGame">
        <div class="fullscreen-text" v-if="screen === 'Start'">
            <h3 class="fullscreen-title">Click to start the game</h3>
            <p>Keep in mind that:</p>
            <ul class="fullscreen-list">
                <li>You have 50 seconds to find every pair</li>
                <li>Each match gives you 5 seconds</li>
            </ul>
        </div>
        <div id="game-over-text" class="fullscreen-text"  v-if="screen === 'Lost'">
            <h3 class="fullscreen-title game-over-title">Game over</h3>
            <p id="game-over-text-small">Click to Restart</p>
        </div>
        <div id="winning-text" class="fullscreen-text" v-if="screen === 'Won'">
            <h3 class="fullscreen-title winning-title">You won!</h3>
            <p id="winning-text-small">Click to play again!</p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['screen'],
    name: 'FullscreenMessage',
    methods: {
        newGame() {
            this.$emit('startGame')
        }
    },
}
</script>

<style>
    html {
        color: #fff;
    }
    .fullscreen-text{
        display: flex;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        z-index: 1000;
        color: rgb(135, 240, 151);
        animation: fullscreen-appear 500ms forwards;
        cursor: pointer;
    }
    @keyframes fullscreen-appear{
    from{
        background-color: rgba(0,0,0,0);
        font-size: 0;
    }
    to{
        background-color: rgba(0,0,0,.9);
        font-size: 2rem;
        }
    }
    .fullscreen-title{
        font-size: 5rem;
        text-transform: capitalize;
        margin-bottom: 2rem;
        text-align: center;
    }
    .fullscreen-list{
        opacity: .8;
        margin-top: .5rem;
    }
    .game-over-title{
        color: #f03e3e;
    }
    @media (max-width: 950px) {
        .fullscreen-title {
            font-size: 3rem;
        }
        .fullscreen-list {
            font-size: 1.2rem;
            max-width: 80%;
            margin: 0 auto;
        }
    }
</style>