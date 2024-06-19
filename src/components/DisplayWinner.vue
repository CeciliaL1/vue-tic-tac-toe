<script setup lang="ts">
import {toRaw, ref} from 'vue'
import { IPlayer } from '../models/Player'
import ButtonComponent  from './ButtonComponent.vue'

interface IStoredData {
    winner: IPlayer[] | undefined;
    storedPlayers: IPlayer[];
    storedGameBoard: [][];
};

const props = defineProps<IStoredData>();
const emits = defineEmits<{
    (e: 'playAgain'):void;
    (e:'newGame'): void;
}>()

const winner = toRaw(props.winner);

console.log(winner)
</script>

<template>
<div>
    <div v-for="win in winner">
        <h1 v-if="win.name != 'Tied game'">Winner is {{ win.name }}</h1>
        <h1 v-else>{{ win.name }}</h1>
        <img :src="win.avatar" alt="">
    </div>


    <div>
        <ButtonComponent buttonText="Play again" buttonEvent="playAgain" @handle-click="() => { $emit('playAgain')}"/>
        <ButtonComponent buttonText="New Game" buttonEvent="newGame" @handle-click="(()=> { $emit('newGame')})"/>
    </div>

    <h3>Score statistics</h3>
    <div class="statistics">
        <div class="player-info" v-for="player in storedPlayers">
            <p> {{ player.name }}</p>
            <p>{{  player.score }}p</p>
            <img :src="player.avatar" alt="">
        </div>
    </div>
</div>
</template>

<style scoped >

img{
    width: 150px;
    height: 150px;
    margin: 30px
}
h3 {
    font-family: 'Yatra One';
}
div .statistics {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
    
}
div .statistics img {
    height: 60px;
    width: 60px;
}
div .player-info {
    display: flex;
    flex-direction: row;
    gap: 20px;
}
.player-info p {
    font-family: Raleway;
    font-weight: 600;
    margin-top: 50px;
}
</style>