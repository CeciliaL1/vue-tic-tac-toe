<script setup lang="ts">
import { ref } from 'vue'


interface IGamePieces {
    chosenPiece: number
}
const gamePieces = ref<IGamePieces[]>([]);

const handleClick = (piece: number) => {
    emit('gamePiece', piece)
    gamePieces.value.push({chosenPiece: piece});

    
};

const emit = defineEmits<{
    (e: 'gamePiece', piece: number):void;
}>();

</script>

<template>
    <div class="game-piece-container">
        <h4>Choose game piece</h4>
        <div class="game-piece-options">
            <div class="game-piece" :class="gamePieces.find((piece) => piece.chosenPiece === 1) ? 'picked-piece': ''" @click="handleClick(1)"><i class="fa-solid fa-x"></i></div>
            <div class="game-piece" :class="gamePieces.find((piece) => piece.chosenPiece === 2) ? 'picked-piece': ''" @click="handleClick(2)"><i class="fa-solid fa-o"></i></div>
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