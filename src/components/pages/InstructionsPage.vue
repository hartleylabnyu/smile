<script setup>
import { useRouter, useRoute } from 'vue-router'
import useTimelineStepper from '@/composables/timelinestepper'
import useSmileStore from '@/stores/smiledata'

const router = useRouter()
const route = useRoute()
const smilestore = useSmileStore()

const { next, prev } = useTimelineStepper()

if(route.meta.progress) smilestore.global.progress = route.meta.progress

function finish(goto) { 
    smilestore.saveData()
    if(goto) router.push(goto)
}
</script>

<template>
    <div class="page">
        <h1 class="title is-3">Instructions</h1>
        <div>
            <p> Today you will be playing a game to earn treasure. </p>
            <p> At the end of the experiment, you will earn a bonus based on how much treasure you find. </p>
            <img src="@/assets/instructions/images/treasure.png">
            <audio autoplay id="music" src="src/assets/instructions/audio/introduction_text_2.wav" onended="finished"></audio>
        </div>
        <button class="button is-light" id='finish' @click="finish(next())">next &nbsp;<FAIcon icon="fa-solid fa-arrow-right" /></button>
    </div>
</template>

<style scoped>
img {
    width: 400px;
    padding: 10px;
}
</style>