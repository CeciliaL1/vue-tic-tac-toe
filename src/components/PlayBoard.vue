<script setup lang="ts">
import { IPlayer } from '../models/Player';

interface IStoredData {
    storedGameBoard: number[][];
    winner: IPlayer[] | undefined;
}

const props = defineProps<IStoredData>();

const handleClick = (i:number, j:number) => {
    emit('handleClick', i, j)
    
};


const emit = defineEmits<{
    (e: 'handleClick', i:number, j:number):void;
    (e: 'handleParentClick', event:Event):void
}>();
</script>

<template>
    
    <div class="container">
        <div class="game-board" v-for="(square, i) in storedGameBoard" :key="i">
            <div class="square" :class="winner ? 'not-clickable': ''" v-for="(sq , j) in square" :key="j" @click="handleClick(i,j)">
                <p class="game-piece" :class="storedGameBoard[i][j] === 1 ? 'piece-X-color' : ''" v-if="storedGameBoard[i][j] === 1">X</p>
                <p class="game-piece" :class="storedGameBoard[i][j] === 2 ? 'piece-O-color' : ''" v-if="storedGameBoard[i][j] === 2">O</p>
            </div>
        </div>
    </div>

</template>

<style scoped>
.container {
    margin-top: 50px;
}
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
.not-clickable {
  pointer-events: none;
  opacity: 0.5;
}
.game-piece{
    margin-top: 15px;
    font-family: 'Yatra One';
    font-size: 3rem;
}
.piece-X-color {
    color: var(--color-darker-green);
}
.piece-O-color {
    color: var(--color-peach)
}
</style>