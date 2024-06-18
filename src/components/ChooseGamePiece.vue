<script setup lang="ts">
import { IPlayer } from '../models/Player'

interface IGameDataProps {
    gamePieces: number[];
    storedPlayers: IPlayer[]
};

const props = defineProps<IGameDataProps>();

const handleClick = (piece: number) => {
    if(props.gamePieces.length === 1){
        return;
    }
    emit('gamePiece', piece);
    props.gamePieces.push(piece);
};

const emit = defineEmits<{
    (e: 'gamePiece', piece: number):void;
}>();

/**
 *  Functions that check picked piece on player in storage and return class-name on click - startgame-btn
 * when player is added to localstorage
 * @param piece - Number that indicates picked piece 1 = X and 2 = O
 */

const checkStoredPiece = (piece: number):string => {
    const storedPiece = props.storedPlayers.find((player) => player.gamePiece === piece);

    if(storedPiece) {
        return 'picked-piece'
    } else {
        return ' ';
    }
}
/**
 *  Functions that check picked piece in game-piece array and add class-name on click piece-btn
 * @param piece - Number that indicates picked piece 1 = X and 2 = O
 */

const checkPickedPiece = (piece: number) => {
    const pickedPiece = props.gamePieces.find((pi) => pi === piece);
    if(pickedPiece) {
        return 'picked-piece'
    } else {
        return ' ';
    }
}

</script>

<template>
    <div class="game-piece-container">
        <h4>Choose game piece</h4>
        <div class="game-piece-options">
            <div class="game-piece" :class="checkStoredPiece(1), checkPickedPiece(1) " @click="handleClick(1)"><i class="fa-solid fa-x"></i></div>
            <div class="game-piece" :class="checkStoredPiece(2), checkPickedPiece(2)" @click="handleClick(2)"><i class="fa-solid fa-o"></i></div>
        </div>
    </div>
</template>

<style scoped>

.game-piece-container h4 {
    margin: 2px;
}
.game-piece-options {
    display: flex;
    flex-direction: row;
   justify-content: center;
}
.game-piece {
   
    background-color: var(--background-color);
    border: none;
    cursor: pointer;
    margin: 10px;
    
}
.game-piece i{
    font-size: 2rem;
}
.game-piece i:hover {
    color: var(--color-peach);
}
.picked-piece {
   display: none;
}
</style>