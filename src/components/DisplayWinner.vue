<script setup lang="ts">
import {toRaw, ref} from 'vue'
import { IPlayer } from '../models/Player'
import ButtonComponent  from './ButtonComponent.vue'

interface IStoredData {
    winner: IPlayer[] | undefined;
    storedPlayers: IPlayer[]
};

const props = defineProps<IStoredData>();
const emits = defineEmits<{
    (e: 'playAgain', value:string):void;
    (e:'newGame', value:string): void;
}>()

const winner = toRaw(props.winner);

</script>

<template>
<div>
    <div v-for="win in winner">
        <h1>Winner is {{ win.name }}</h1>
        <img :src="win.avatar" alt="">
    </div>
  
    <div>
        <ButtonComponent buttonText="Play again" buttonEvent="playAgain" @handle-click="(value: string) => { $emit('playAgain',value)}"/>
        <ButtonComponent buttonText="New Game" buttonEvent="newGame" @handle-click="((value: string)=> { $emit('newGame',value)})"/>
    </div>
    <h4>Score statistics</h4>
    <div v-for="player in storedPlayers">
        <p> {{ player.name }} {{ player.score }}</p>
    </div>
</div>

</template>

<style scoped >
img{
    width: 150px;
    height: 150px;
}

</style>