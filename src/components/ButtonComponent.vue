<script setup lang="ts">
import {ref} from 'vue'

interface IButtonProps {
    buttonText: string;
    buttonEvent: string;
}

const props = defineProps<IButtonProps>();

const click = ref<IButtonProps>(props);

const emit = defineEmits<{
    (e: 'handleClick', value:string):void
}>();

</script>


<template>
<button @click="$emit('handleClick', click.buttonEvent) ">{{ buttonText }}</button>
</template>

<style scoped>
button {
    font-family: Raleway;
    height: 40px;
    width: 300px;
    background-color: var(--color-darker-green);
    border: none;
    border-radius: 10px;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    font-size: 0.7rem;
    font-weight: 700;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    cursor: pointer;
    color: black;
}
button:hover{
    background-color: var(--hover-color);
}
</style>