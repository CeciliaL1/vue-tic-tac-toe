<script setup lang="ts">
import GameBoard from '../components/GameBoard.vue'
import InputPlayer from './InputPlayer.vue'
import type { IPlayer } from '../models/Player'
import { onMounted, ref } from 'vue'

let players: IPlayer[] = [];
let enableGame = ref('enableGame');


const storedPlayers:IPlayer[] = JSON.parse(localStorage.getItem('players') || '{}')

onMounted(() => {
        
    if(storedPlayers.length === 2) {
        //start Game
        enableGame.value = '';
    }
})
    //localStorage.clear()



const startGame = (player: IPlayer) => {
    const gameBoard = [0,0,0 ,0,0,0 ,0,0,0];

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



</script>

<template>

<div class="content" v-if="enableGame === 'enableGame'">
    <InputPlayer  @start-game="startGame"/> 
</div>
<div class="content"  v-if="enableGame === ''">
    <GameBoard />
</div>


</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}

</style>
