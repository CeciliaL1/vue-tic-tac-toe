<script setup lang="ts">
import { ref } from 'vue';
import type { IPlayer } from '../models/Player' 
import ButtonCompenent from './ButtonComponent.vue'
import ChooseGamePiece from './ChooseGamePiece.vue'
import ChooseAvatar from './ChooseAvatar.vue'

interface IStoredPlayers {
    storedPlayers: IPlayer[]
};

const props = defineProps<IStoredPlayers>();

let gamePieces = ref<number[]>([]);
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
    gamePieces.value = [];
};

const emit = defineEmits<{
    (e: 'startGame', player: IPlayer):void
}>();

</script>

<template>
    <form @submit.prevent="handleSubmit">
        <div class="form-left">
            <h4 v-if="storedPlayers.length === 0">Player 1 name</h4>
            <h4 v-else>Player 2 name</h4>
            <input type="text" v-model="playerName">
            <ChooseGamePiece :storedPlayers="storedPlayers" :gamePieces="gamePieces" @game-piece="handlePiece"/>
        </div>

       <div class="form-right">
            <ChooseAvatar @chose-avatar="handleAvatar" />
       </div>

       <div class="players-presentation">
            <div class="player-info" v-for="player in storedPlayers">
                <p> {{ player.name }}</p>
                <p><span v-if=" player.gamePiece === 1">X</span><span v-if=" player.gamePiece === 2 "> O</span></p>
                <img :src="player.avatar" alt="">
            </div>
        </div>
        <ButtonCompenent  :buttonText="storedPlayers.length === 0 ? 'Create Player 1' : 'Start game'"/>
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
div .players-presentation {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
    
}
div .players-presentation img {
    height: 60px;
    width: 60px;
    margin-top: 25px;
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