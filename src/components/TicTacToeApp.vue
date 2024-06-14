<script setup lang="ts">
import ButtonCompenent from './ButtonComponent.vue';
import InputPlayer from './InputPlayer.vue'
import type { IPlayer } from '../models/Player'
import { ref } from 'vue'

let players: IPlayer[] = []
let enableGame = ref('enableGame');

const init = () => {
    //localStorage.clear()
    const storedPlayers:IPlayer[] = JSON.parse(localStorage.getItem('players') || '{}')
    if(storedPlayers.length === 2) {
        //start Game
        enableGame.value = '';
        console.log(enableGame)
    }
}

const startGame = (player: IPlayer) => {
    console.log('starta spel', player)
    players.push(player)
    localStorage.setItem('players', JSON.stringify(players));

    const storedPlayers: IPlayer[] = JSON.parse(localStorage.getItem('players') || '{}')
    console.log('checka', storedPlayers.length)
    if(storedPlayers.length === 2) {
        enableGame.value = '';
    }
}


init()
</script>

<template>
<div class="content" v-if="enableGame === 'enableGame'">
    <InputPlayer  @start-game="startGame" /> 
</div>
<div class="content"  v-if="enableGame === ''">
    <div>{{ players }}</div>
</div>


</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}

</style>
