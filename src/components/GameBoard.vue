<script setup lang="ts">
import { ref, toRaw } from 'vue';
import type { IPlayer } from '../models/Player'

interface IStoredData {
    storedPlayers: IPlayer[];
    storedGameBoard: number[][];
}

const props = defineProps<IStoredData>();


const winningOptions = ref([
        [[0, 0], [0, 1], [0, 2]],
        [[1, 0], [1, 1], [1, 2]],
        [[2, 0], [2, 1], [2, 2]],

        [[0, 0], [1, 0], [2, 0]],
        [[0, 1], [1, 1], [2, 1]],
        [[0, 2], [1, 2], [2, 2]],

        [[0, 0], [1, 1], [2, 2]],
        [[0, 2], [1, 1], [2, 0]]
]);

let didSomeOneWin = ref(false);
let winner = ref<IPlayer[]>();

const emit = defineEmits<{
    (e: 'checkValues', didSomeOneWin: boolean, winnerValue:IPlayer[] | undefined):void
}>();

const handleClick = (i:number, j:number) => {
    console.log('0', props.storedPlayers[0])
    console.log('1', props.storedPlayers[1])
    if(props.storedGameBoard[i][j] === 0 ) {
        
    if(props.storedPlayers[0].count === props.storedPlayers[1].count){
        props.storedPlayers[0].count ++;
        props.storedGameBoard[i][j] = props.storedPlayers[0].gamePiece;
        checkBoardResult(props.storedPlayers[0]);

    } else {
        props.storedPlayers[1].count ++;
        props.storedGameBoard[i][j] = props.storedPlayers[1].gamePiece;
        checkBoardResult(props.storedPlayers[1]);

    }
    localStorage.setItem('winner', JSON.stringify(winner.value));
    localStorage.setItem('players', JSON.stringify(props.storedPlayers));
    localStorage.setItem('gameBoard', JSON.stringify(props.storedGameBoard));
    emit('checkValues', didSomeOneWin.value, winner.value);
    
    }else {
        return;
    };
   
};

const checkBoardResult = (player:IPlayer) => {
    const checkedWinner = checkWinner();
    const array = toRaw(props.storedGameBoard)

    if(checkedWinner){
        didSomeOneWin.value = true;
        winner.value = [player];
       player.score ++; 

    } else if (!checkedWinner && !array[0].includes(0) && !array[1].includes(0) && !array[2].includes(0)) {
        didSomeOneWin.value = true;
        winner.value = [{name:'Tied game', gamePiece: 0, avatar: '/tie-svgrepo-com.svg', count:0, score:0}];
    }
};
const checkWinner = () => {
    for (const combination of winningOptions.value) {
        const [a, b, c] = combination;

        if (props.storedGameBoard[a[0]][a[1]] &&
            props.storedGameBoard[a[0]][a[1]] === props.storedGameBoard[b[0]][b[1]] &&
            props.storedGameBoard[a[0]][a[1]] === props.storedGameBoard[c[0]][c[1]]
        ) {
            return true;
        }
    }
    return false;
}

</script>

<template>
<div class="players-presentation">
    <div v-for="(player,i) in storedPlayers">
        <p>{{ player.name }}   <span v-if=" player.gamePiece === 1 "> X</span><span v-if=" player.gamePiece === 2 "> O</span></p>
       
        <p> Points {{ storedPlayers[i].score}}</p>
        <img :src="player.avatar" alt="">
    </div>
</div>

<div v-if="!didSomeOneWin" >
    <p v-if="storedPlayers[0].count === storedPlayers[1].count">Players turn {{ storedPlayers[0].name }}</p>
    <p v-if="storedPlayers[0].count > storedPlayers[1].count">Players turn {{ storedPlayers[1].name }} </p>
 
<div class="game-board" v-for="(square, i) in storedGameBoard" >
<div class="square" v-for="(sq , j) in square" :key="j" @click="handleClick(i, j)">
    <p class="game-piece" v-if="storedGameBoard[i][j] === 1">X</p>
    <p class="game-piece" v-if="storedGameBoard[i][j] === 2">O</p>
</div>

</div>
</div>
</template>

<style scoped>
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
.players-presentation {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 100%;
    height: 100px;
    background-color: var(--color-darker-green);
    margin-top: 50px;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.players-presentation p {
    font-family: 'Yatra One';
    margin: 2px;
}
.players-presentation img {
    width: 40px;
    height: auto;
}
.game-piece{
    margin-top: 15px;
    font-family: 'Yatra One';
    font-size: 3rem;
}

</style>