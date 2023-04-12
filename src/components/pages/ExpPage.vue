<script setup>
import { onKeyStroke } from '@vueuse/core'
import { reactive } from 'vue' 
import { useRouter, useRoute } from 'vue-router'
import {sampleWithReplacement} from '@/randomization'
import useTimelineStepper from '@/composables/timelinestepper'
import useSmileStore from '@/stores/smiledata' // get access to the global store

const router = useRouter()
//const route = useRoute()
const smilestore = useSmileStore()
//if(route.meta.progress) smilestore.global.progress = route.meta.progress
const { next, prev } = useTimelineStepper()

// set up trials
var testStimuli = ["./src/assets/blue.png", "./src/assets/orange.png"];
var fixationDurations = [250, 500, 750, 1000, 1250, 1500, 1750, 2000];

// get 10 random stimuli
var stimuli = sampleWithReplacement(testStimuli, 10)

// get 10 random durations
var durations = sampleWithReplacement(fixationDurations, 10)

// generate array of 10 trials, with random stimulus
var trials = []
for (let i = 0; i < 10; i++) {
  trials.push({
    trial: i,
    stimulus: stimuli[i],
    fixation_duration: durations[i],
    response: null
  })
}

// REACTIVE object tracking the current trial
const currentTrial = reactive({trial: 0})

// REACTIVE object tracking fixation (on/off)
const fixationOn = reactive({value: true})

// set a timeout to turn off fixation
setTimeout(()=>{
    fixationOn.value = false
}, trials[currentTrial.trial].fixation_duration);

// what happens when f or j is pressed
onKeyStroke(['f', 'j'], (e) => {
    e.preventDefault()

    // record the keypress in response
    trials[currentTrial.trial].response = e.key

    // go to the next trial
    currentTrial.trial += 1

    if (currentTrial.trial >= trials.length){
        smilestore.setTaskData(trials)
        router.push(next())
    }
    else {
        // turn fixation back on
        fixationOn.value = true
        setTimeout(()=>{
            fixationOn.value = false
        }, trials[currentTrial.trial].fixation_duration);
    }
})

</script>

<template>
    <div class="page">
        <h1 class="title is-3">Experiment</h1>
        <div class="trial">
            <template v-for="trial in trials" :key="trial.trial">
            <div v-if="trial.trial == currentTrial.trial && fixationOn.value" class="fixation">
                <p>+</p>
            </div>
            <img v-if="trial.trial == currentTrial.trial && !fixationOn.value" :src="trial.stimulus"/>
            </template>
        </div>
    </div>
</template>

<style scoped>
.trial{
    margin:80px;
    height:250px;
    position: relative;
}
.fixation {
    font-size: 3em;
    margin: 0;
    position: absolute;
    top: 50%;
    left: 50%;
    width: auto;
    -ms-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
}

img {
    width: 250px;
}
</style>
