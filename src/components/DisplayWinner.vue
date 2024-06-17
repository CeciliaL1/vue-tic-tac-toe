<script setup lang="ts">
import {toRaw} from 'vue'
import { IPlayer } from '../models/Player'
import ButtonComponent  from './ButtonComponent.vue'

interface IWinnerProps {
    player: IPlayer[] | undefined
}

const props = defineProps<IWinnerProps>();
const emits = defineEmits<{
    (e: 'playAgain', value:string):void;
    (e:'newGame', value:string): void;
}>()

const winner = toRaw(props.player)

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
    
</div>

</template>

<style scoped >
img{
    width: 150px;
    height: 150px;
}

</style>