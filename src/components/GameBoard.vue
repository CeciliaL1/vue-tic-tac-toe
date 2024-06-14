<script setup lang="ts">
import { onMounted, ref } from 'vue';
import type { IPlayer } from '../models/Player'

//spara gameboard i localstorage , checkBoard på localstorage istället
const gameBoard = [0,0,0, 0,0,0, 0,0,0];

const storedPlayers:IPlayer[] = JSON.parse(localStorage.getItem('players') || '{}')

onMounted(() => {
    const checkBoard = (gameBoard.find((item) => item > 0)  || '')
    console.log(checkBoard)
  
    if ( checkBoard === 1 ) {
        return
    } else {
        const shuffledArray = storedPlayers.sort(() => Math.random() - 0.5)
        startingPlayer.value = shuffledArray[0].name
        startingPlayerGamePiece.value = shuffledArray[0].gamePiece
  
    }
    
});


const handleClick = (e: Event) => {
    console.log(e)
}


let startingPlayer = ref('')
let startingPlayerGamePiece = ref('')
 

</script>


<template>
    <p>Player starting is {{ startingPlayer }} {{ startingPlayerGamePiece }}</p>
<div class="game-board">
<div class="square"  v-for="square in gameBoard" @click="handleClick">
</div>
</div>

<div class="players-presentation">
    <div v-for="player in storedPlayers">
        <p>{{ player.name }} {{ player.gamePiece }}</p>
        <img :src="player.avatar" alt="">
    </div>
</div>
</template>

<style scoped>
.game-board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    border: 1px solid black;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
   
}
.square {
    width: 100px;
    height: 100px;
    border: 1px solid black;
    cursor: pointer;
    background-color: #f8f8f8
}
.square:hover {
    background-color: var(--color-yellow);
    opacity: 70%;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.players-presentation {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 100%;
    height: 70px;
    background-color: var(--color-darker-green);
    margin-top: 50px;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.players-presentation p {
    font-family: 'Yatra One';
    margin: 2px;
}
.players-presentation img {
    width: 40px;
    height: auto;
}

</style>