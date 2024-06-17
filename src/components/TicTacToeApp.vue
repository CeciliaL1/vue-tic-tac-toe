<script setup lang="ts">
import GameBoard from '../components/GameBoard.vue'
import InputPlayer from './InputPlayer.vue'
import type { IPlayer } from '../models/Player'
import {  ref, toRaw } from 'vue'
import DisplayWinner from './DisplayWinner.vue'

let players: IPlayer[] = [];
let enableGame = ref('enableGame');
let winner = ref<IPlayer[]>()


const storedPlayers = ref<IPlayer[]>(JSON.parse(localStorage.getItem('players') || '{}'));


    const gameBoard = [
        [0,0,0],
        [0,0,0],
        [0,0,0]
    ]
   
const getStoredWinner = () => {
    const storedData = localStorage.getItem('winner');
    if(!storedData) {
    return [];
}
try {
    return JSON.parse(storedData) as IPlayer[];
  } catch (error) {
    return [];
  }
}

const  init =() => {
    const storedWinner = ref<IPlayer[]>(getStoredWinner())
        console.log(storedWinner.value)
        if(storedPlayers.value.length === 2) {
            localStorage.setItem('gameBoard', JSON.stringify(gameBoard));
            //start Game
            enableGame.value = '';
        }
    
        if(storedWinner.value.length === 1) {
            enableGame.value = 'isAWinner'
            winner.value = toRaw(storedWinner.value)
            
        }

    }

    //localStorage.clear()

const startGame = (player: IPlayer) => {
 
    localStorage.setItem('gameBoard', JSON.stringify(gameBoard));

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
 localStorage.clear();
 players = [];
enableGame.value = 'enableGame';
}

</script>

<template>
<div v-if="enableGame === 'isAWinner'">
    <DisplayWinner :player="winner" @play-again="playAgain" @new-game="newGame"/>
</div>

<div class="content" v-if="enableGame === 'enableGame'">
    <InputPlayer  @start-game="startGame" :player="storedPlayers"/> 
</div>
<div class="content"  v-if="enableGame === '' ">
    <GameBoard @check-values="checkValues"/>
</div>


</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}

</style>
