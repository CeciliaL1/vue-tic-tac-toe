<script setup lang="ts">
import GameBoard from '../components/GameBoard.vue'
import InputPlayer from './InputPlayer.vue'
import type { IPlayer } from '../models/Player'
import {  onMounted, ref, toRaw } from 'vue'
import DisplayWinner from './DisplayWinner.vue'


let players: IPlayer[] = [];
let enableGame = ref('enableGame');
let winner = ref<IPlayer[]>();
let storedPlayers = ref<IPlayer[]>(JSON.parse(localStorage.getItem('players') || '[]'));
let storedGameBoard = ref<[][]>(JSON.parse(localStorage.getItem('gameBoard') || '[]'));

const gameBoard = [
    [0,0,0],
    [0,0,0],
    [0,0,0]
];

onMounted(()=> {
    const storedWinner = ref<IPlayer[]>(getStoredWinner())
    if(storedPlayers.value.length === 2) {
        enableGame.value = '';
    }
    
        if(storedWinner.value.length === 1) {
            enableGame.value = 'isAWinner'
            winner.value = toRaw(storedWinner.value)
        }
});

const getStoredWinner = () => {
    const storedData = localStorage.getItem('winner');
    if(!storedData) {
    return [];
};

try {
    return JSON.parse(storedData) as IPlayer[];
  } catch (error) {
    return [];
  }
};

const startGame = (player: IPlayer) => {
    localStorage.setItem('gameBoard', JSON.stringify(gameBoard));

    players.push(player)
    localStorage.setItem('players', JSON.stringify(players));

    storedPlayers.value = JSON.parse(localStorage.getItem('players') || '[]');
    storedGameBoard.value = JSON.parse(localStorage.getItem('gameBoard') || '[]')

    if(storedPlayers.value.length === 2) {
        setTimeout(() => {
            enableGame.value = ''
        },1000);
    };
};



const checkValues = (didSomeOneWin: boolean, winnerValue: IPlayer[]| undefined) => {
    if(didSomeOneWin) {
        enableGame.value = 'isAWinner';
        winner.value = winnerValue;
        localStorage.removeItem('gameBoard');
    };
};

const playAgain = () =>{
    localStorage.removeItem('winner');
    localStorage.setItem('gameBoard', JSON.stringify(gameBoard));
    enableGame.value = '';
    storedGameBoard.value = JSON.parse(localStorage.getItem('gameBoard') || '[]');
};

const newGame = () => {
    localStorage.clear();
    players = [];
    enableGame.value = 'enableGame';
    storedPlayers.value = [];
    storedGameBoard.value = [];
}
</script>

<template>
<div v-if="enableGame === 'isAWinner'">
    <DisplayWinner :storedPlayers="storedPlayers" :winner="winner" @play-again="playAgain" @new-game="newGame"/>
</div>

<div class="content" v-if="enableGame === 'enableGame'">
    <InputPlayer  @start-game="startGame" :storedPlayers="storedPlayers"/> 
</div>
<div class="content"  v-if="enableGame === '' ">
    <GameBoard :storedGameBoard="storedGameBoard" :storedPlayers="storedPlayers" @check-values="checkValues"/>
</div>
</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}
</style>
