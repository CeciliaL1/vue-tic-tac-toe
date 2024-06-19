<script setup lang="ts">

const avatars:string[] = ['/elephant.svg', '/giraffe.svg', '/gorilla.svg', '/pinguin.svg', '/rhino.svg', '/tiger.svg'];


const handleClick = (animal:string, e: Event ):void => {
emit('choseAvatar', animal);
colorBackground(e);
};

const colorBackground = (e:Event):void => {
const event = (e.target as Element);
event.classList.add('chosen-avatar');
}

const emit = defineEmits<{
    (e: 'choseAvatar', animal:string):void
}>();
</script>

<template>
    <h4>Choose an avatar</h4>
<div class="avatar-container" >
    <img v-for="(animal, i) in avatars" :src="animal" alt="" :key="i" @click="handleClick(animal, $event)">
  
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
    border-radius: 10px;
}

img:hover{
    background-color: var(--color-peach);
}
img.chosen-avatar {
    background-color: var(--color-yellow);
}
</style>