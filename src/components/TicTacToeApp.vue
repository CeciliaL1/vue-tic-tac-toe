<script setup lang="ts">
import ButtonCompenent from './ButtonComponent.vue';
import InputPlayer from './InputPlayer.vue'
import type { Iplayer } from '../models/Player'
import { ref } from 'vue'

let player = localStorage.getItem('player')

let clickValue = ref('');

const startGame = (playerName:string) => {
    console.log('starta spel', playerName)
    //spara spelare till localStorage
}
</script>

<template>
<div class="content">
    <InputPlayer @start-game="startGame" />
</div>

</template>

<style scoped>
.content {
    position: absolute;
    top: 150px;
}
</style>
