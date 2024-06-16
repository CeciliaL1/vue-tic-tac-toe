<script setup lang="ts">
import { onMounted, ref } from 'vue';
import type { IPlayer } from '../models/Player'

const storedPlayers= ref<IPlayer[]>(JSON.parse(localStorage.getItem('players') || '{}'));
const storedGameBoard = JSON.parse(localStorage.getItem('gameBoard') || '');



const handleClick = (i:number) => {
    
    if(storedPlayers.value[0].count === storedPlayers.value[1].count){
        storedPlayers.value[0].count ++;
        storedGameBoard[i] = storedPlayers.value[0].gamePiece;
        
    } else {
        storedPlayers.value[1].count ++;
        storedGameBoard[i] = storedPlayers.value[1].gamePiece;
    }

 
    localStorage.setItem('players', JSON.stringify(storedPlayers.value));
    localStorage.setItem('gameBoard', JSON.stringify(storedGameBoard));
}

</script>

<template>
    <div class="players-presentation">
    <div v-for="player in storedPlayers">
        <p>{{ player.name }} {{ player.gamePiece }}</p>
        <img :src="player.avatar" alt="">
    </div>
</div>
    <p v-if="storedPlayers[0].count === storedPlayers[1].count">Players turn {{ storedPlayers[0].name }} {{ storedPlayers[0].gamePiece }}</p>
    <p v-if="storedPlayers[0].count > storedPlayers[1].count">Players turn {{ storedPlayers[1].name }} {{ storedPlayers[1].gamePiece }}</p>
<div class="game-board"> 
<div class="square"  v-for="(square , i) in storedGameBoard" :key="i"  @click="handleClick(i)" >
    <p v-if="storedGameBoard[i] === 'X'">X</p>
    <p v-if="storedGameBoard[i] === 'O'">O</p>
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
.square .clicked {
    background-color: aqua;
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