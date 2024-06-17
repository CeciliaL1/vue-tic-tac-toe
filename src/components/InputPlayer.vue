<script setup lang="ts">
import { ref } from 'vue';
import type { IPlayer } from '../models/Player' 
import ButtonCompenent from './ButtonComponent.vue'
import ChooseGamePiece from './ChooseGamePiece.vue'
import ChooseAvatar from './ChooseAvatar.vue'

let playerName = ref('');
let gamePiece = ref(0);
let avatar = ref('');

const handlePiece = (piece: number) => {
    gamePiece.value = piece;
};

const handleAvatar = (animal:string) => {
    avatar.value = animal;
};

const handleSubmit = () => {
    if(playerName.value === '' || gamePiece.value === 0 ) {
       return;
    };
    
    const player: IPlayer = {
        name: playerName.value,
        gamePiece: gamePiece.value,
        avatar: avatar.value,
        score: 0,
        count: 0
    };
    emit('startGame', player);
    playerName.value = '';
};

const emit = defineEmits<{
    (e: 'startGame', player: IPlayer):void
}>();

</script>

<template>
    <form @submit.prevent="handleSubmit">
        <div class="form-left">
            <h4>Players name</h4>
            <input type="text" v-model="playerName">
            <ChooseGamePiece @game-piece="handlePiece"/>
        </div>
       <div class="form-right">
        <ChooseAvatar @chose-avatar="handleAvatar" />
       </div>
        
        <ButtonCompenent buttonText="Start game" buttonEvent="startGame"/>
      
    </form>
</template>

<style scoped >

form {
    font-family: 'Yatra One';
    display: grid;
    grid-template-columns: repeat(1, 1fr); 
    gap: 20px;
}
form h4 {
    margin: 2px;
}
form p {
    color: var(--error-message-color);
}
form input {
    height: 40px;
    width: 300px;
    border-radius: 10px;
    background-color: var(--color-peach);
    border: none;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    margin-bottom: 10px;
    cursor: pointer;
    outline: none;
}
form input:hover {
    background-color: var(--color-ligher-green);
    opacity: 70%;
}


</style>