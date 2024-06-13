<script setup lang="ts">
import { ref } from 'vue';
import ButtonCompenent from './ButtonComponent.vue'
import ChooseGamePiece from './ChooseGamePiece.vue'

let playerName = ref('')
 
const handleSubmit = () => {
    if(playerName.value === '') return
    emit('startGame', playerName.value)
    playerName.value = ''
}

const emit = defineEmits<{
    (e: 'startGame', playerName:string):void
}>();

</script>

<template>
    <form @submit.prevent="handleSubmit">
        <h4>Players name</h4>
        <input type="text" v-model="playerName">
        <ChooseGamePiece />
        <ButtonCompenent buttonText="Start game" buttonEvent="startGame"/>
    </form>
</template>

<style scoped >

form {
    font-family: 'Yatra One';
}
form h4 {
    margin: 2px;
}
</style>