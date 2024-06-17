<script setup lang="ts">
import GameBoard from '../components/GameBoard.vue'
import InputPlayer from './InputPlayer.vue'
import type { IPlayer } from '../models/Player'
import {  onMounted, ref } from 'vue'
import DisplayWinner from './DisplayWinner.vue'

let players: IPlayer[] = [];
let enableGame = ref('enableGame');
let winner = ref<IPlayer[]>()


const storedPlayers = ref<IPlayer[]>(JSON.parse(localStorage.getItem('players') || '{}'));
const storedWinner = ref<IPlayer[]>(JSON.parse(localStorage.getItem('winner') || '{}'))

    const gameBoard = [
        [0,0,0],
        [0,0,0],
        [0,0,0]
    ]
   

const  init =() => {
    
        if(storedPlayers.value.length === 2) {
            localStorage.setItem('gameBoard', JSON.stringify(gameBoard));
            //start Game
            enableGame.value = '';
        }

        if(storedWinner.value.length === 1) {
            enableGame.value = 'isAWinner'
        }

    }

    //localStorage.clear()

const startGame = (player: IPlayer) => {
 
    localStorage.setItem('gameBoard', JSON.stringify(gameBoard));
    console.log('starta spel', player)
    players.push(player)
    localStorage.setItem('players', JSON.stringify(players));

    const storedPlayers: IPlayer[] = JSON.parse(localStorage.getItem('players') || '{}')
    console.log('checka', storedPlayers.length)
    if(storedPlayers.length === 2) {
        setTimeout(() => {
            enableGame.value = ''
        },2000);
    }
}

init()
const checkValues = (didSomeOneWin: boolean, winnerValue: IPlayer[]| undefined) => {
if(didSomeOneWin) {
    enableGame.value = 'isAWinner'
    winner.value = winnerValue
    localStorage.removeItem('gameBoard')
}
}
const playAgain = () =>{
    localStorage.removeItem('winner')
    localStorage.setItem('gameBoard', JSON.stringify(gameBoard));
    

    enableGame.value = '';

  
};
const newGame = () => {
    localStorage.removeItem('players')
    localStorage.removeItem('winner')
    localStorage.removeItem('gameBoard')
    enableGame.value = 'enableGame';
}

</script>

<template>
<div v-if="enableGame === 'isAWinner'">
    <DisplayWinner :player="winner" @play-again="playAgain" @new-game="newGame"/>
</div>

<div class="content" v-if="enableGame === 'enableGame'">
    <InputPlayer  @start-game="startGame"/> 
</div>
<div class="content"  v-if="enableGame === '' ">
    <GameBoard @check-values="checkValues" />
</div>


</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}

</style>
