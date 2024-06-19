<script setup lang="ts">
import { ref, toRaw } from 'vue';
import type { IPlayer } from '../models/Player'
import PlayBoard from './PlayBoard.vue'

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
    <div class="player-turn" v-if="!didSomeOneWin" >
        <p v-if="storedPlayers[0].count === storedPlayers[1].count">Players turn {{ storedPlayers[0].name }} 
            <span v-if=" storedPlayers[0].gamePiece === 1">X</span><span v-if=" storedPlayers[0].gamePiece === 2 "> O</span>
        </p>
        <p v-if="storedPlayers[0].count > storedPlayers[1].count">Players turn {{ storedPlayers[1].name }} 
            <span v-if=" storedPlayers[1].gamePiece === 1">X</span><span v-if=" storedPlayers[1].gamePiece === 2 "> O</span>
        </p>
    </div>

    <div class="players-presentation">
        <div v-for="(player,i) in storedPlayers">
            <p>{{ player.name }}   
                <span v-if=" player.gamePiece === 1">X</span>
                <span v-if=" player.gamePiece === 2 "> O</span>
            </p>
            <p> Points {{ storedPlayers[i].score}}</p>
            <img :src="player.avatar" alt="">
        </div>
    </div>

    <PlayBoard :storedGameBoard="storedGameBoard" @handle-click="handleClick" />
</template>

<style scoped>

.players-presentation {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 100%;
    height: 100px;
    background-color: var(--color-darker-green);
    margin-top: 30px;
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
.player-turn {
    text-transform: uppercase;
    padding: 3px;
    font-family: 'Yatra One';
    font-weight: 700px;
    font-size: 1rem;
    background-color: var(--color-yellow);
    border-radius: 10px;
    margin-bottom: 3px
}


</style>