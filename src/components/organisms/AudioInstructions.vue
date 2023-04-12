<script setup>
import { defineEmits, defineProps, ref } from 'vue'
import useSmileStore from '@/stores/smiledata'

const props = defineProps(['audio','image','text'])
const emit = defineEmits(['next'])

const smileStore = useSmileStore()
const activateNext = ref(props.audio ? false: true)

function next(){
    smileStore.saveData()
    emit('next')
}

function audioEnded(){
    activateNext.value = true
}
</script>


<template>
    <div>
        <p v-for="t in text"> {{ t }} </p>
        <img v-if="image" :src="image">
        <audio v-if="audio" autoplay id="music" :src="audio" @ended="audioEnded()"></audio>
    </div>
    <button class="button is-success is-light" v-if="this.activateNext" id='finish' @click="next">next &nbsp;<FAIcon icon="fa-solid fa-arrow-right" /></button>
</template>

<style scoped>
img {
    width: 400px;
    padding: 10px;
    margin: 20px;
}
button {
    margin: 20px;
}
</style>