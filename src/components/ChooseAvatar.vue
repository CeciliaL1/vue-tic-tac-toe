<script setup lang="ts">

const avatars =['/src/assets/Avatars/elephant.svg', '/src/assets/Avatars/giraffe.svg', '/src/assets/Avatars/gorilla.svg', '/src/assets/Avatars/pinguin.svg', '/src/assets/Avatars/rhino.svg', '/src/assets/Avatars/tiger.svg'];

const handleClick = (animal:string) => {
emit('choseAvatar', animal);
};

const emit = defineEmits<{
    (e: 'choseAvatar', animal:string):void
}>();
</script>

<template>
    <h4>Choose an avatar</h4>
<div class="avatar-container" >
    <img v-for="animal in avatars" :src="animal" alt="" @click="handleClick(animal)">
</div>
</template>

<style scoped>
.avatar-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
    gap: 10px;

}
img {
    height: 50px;
    width: 50px;
    cursor: pointer;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
      
    padding: 10px;
    border-radius: 10px
;}
</style>