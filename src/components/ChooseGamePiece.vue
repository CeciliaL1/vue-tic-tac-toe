<script setup lang="ts">
import { IPlayer } from '../models/Player'

interface IGameDataProps {
    gamePieces: number[];
    storedPlayers: IPlayer[]
};

const props = defineProps<IGameDataProps>();

const handleClick = (piece: number):void => {
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
 *  Functions that check picked piece on player in storage and in gamePieces array and return class-name.
 * when player is added to localstorage
 * @param piece - Number that indicates picked piece 1 = X and 2 = O
 */

const checkStoredPiece = (piece: number):string => {
    const storedPiece = props.storedPlayers.find((player) => player.gamePiece === piece);
    const pickedPiece = props.gamePieces.find((pi) => pi === piece);

    if(storedPiece || pickedPiece) {
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
            <div class="game-piece" :class="checkStoredPiece(1) " @click="handleClick(1)"><i class="fa-solid fa-x"></i></div>
            <div class="game-piece" :class="checkStoredPiece(2)" @click="handleClick(2)"><i class="fa-solid fa-o"></i></div>
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