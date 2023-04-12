<script setup>
import { ref } from 'vue'
import AudioInstructions from '../organisms/AudioInstructions.vue';
import useTimelineStepper from '@/composables/timelinestepper'
import { useRouter, useRoute } from 'vue-router'

const data = [
    {
        text: [
            "Today you will be playing a game to earn treasure.", 
            "At the end of the experiment, you will earn a bonus based on how much treasure you find."
        ],
        audio: "src/assets/instructions/audio/introduction_text_2.wav",
        image: "src/assets/instructions/images/treasure.png"
    },
    {
        text: [
            "Animals have hidden treasure in treasure chests.", 
            "You need to first find an animal and then choose which of their treasure chests to search."
        ],
        audio: "src/assets/instructions/audio/introduction_text_3.wav",
        image: "src/assets/instructions/images/cat.jpg"
    }
]

const { next, prev } = useTimelineStepper()
const router = useRouter()
const route = useRoute()
if(route.meta.progress) smilestore.global.progress = route.meta.progress


const currentStep = ref(0)
function step() {
    currentStep.value += 1
    if (currentStep.value >= data.length){
        router.push(next())
    }
}

</script>

<template>
    <div class="page">
        <h1 class="title is-3">Instructions</h1>
        <div v-for="(i,key) in data">
            <AudioInstructions v-if="key==currentStep" v-bind="i" @next="step()"></AudioInstructions>
        </div>
    </div>
</template>

<style scoped>
img {
    width: 400px;
    padding: 10px;
}
</style>