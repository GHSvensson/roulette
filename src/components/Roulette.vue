<template>
<VueWinWheel ref="winWheel" :options="allOptions" :canvasWidth="canvasWidth" :canvasHeight="canvasHeight" :stopAt="stopAt" />

    <button @click="spinWinWheel">Spins</button>>
    <button @click="spinWinWheelStopAt">Skip Animation</button>
</template>

<script setup>
import { VueWinWheel } from "@zohaibtariq/vuewinwheel";
import '@zohaibtariq/vuewinwheel/dist/style.css'
import { ref,onMounted } from "vue";
import segments from './assets/segments.json'

const winWheel = ref();

const spinWinWheel = () => {
  spinStartTime = Date.now() // if you miss this sound will not play after first run
  if(winWheel.value)
    winWheel.value.spin() // winWheelInstances is only required for the (2 part wheel)
};

const spinWinWheelStopAt = () => {
  spinStartTime = Date.now() // if you miss this sound will not play after first run
  if(winWheel.value)
    winWheel.value.spinAndStopAt(stopAt)
};

const canvasWidth = 500; // set custom canvas width
const canvasHeight = 500; // set custom canvas height
const stopAt = 0; // Segment at which you want to stop at

//let spinningAudio = new Audio('./spinning.mp3'); // update this with your own tik sound
//let winnerAudio = new Audio('./winner.mp3'); // update this with your own win sound

// Called when the animation has finished.
function spinStopped(indicatedSegment)
{

    console.log("You have won " + indicatedSegment.text);
}

function eachSegmentSpin()
{
  playSpinningSound()
}

let spinStartTime;

function playSpinningSound()
{
  if(!spinStartTime){
    spinStartTime = Date.now(); // this must start when spin starts
  }
  const spinEndTime = Date.now();
  const diffInSpinTime = spinEndTime - spinStartTime
  // do not play this sound if animation time is exceeded
  if(options?.animation?.duration === undefined || diffInSpinTime < ((options.animation.duration * 1000)/1.4)) { // if you observe wheel stop but tik sound continues to play increase value of 1.4 to gradually a higher digit number like 1.5, 1.6 what ever suites you, animation duration and spin setting effects it
    spinningAudio.currentTime = 0;
  }
}


const options = { // basic_code_wheel
  'numSegments'  : segments.length,            // Number of segments
  'outerRadius'  : 212,        // The size of the wheel.
  'centerX'      : 217,        // Used to position on the background correctly.
  'centerY'      : 219,
  'textFontSize' : 28,        // Font size.
  'textFillStyle' : 'white',
  'segments'     :   segments,           // Definition of all the segments.
  
  'animation' :                // Definition of the animation
      {
        'type'     : 'spinToStop',
        'duration' : 5,
        'spins'    : 8,
        'callbackFinished' : spinStopped,  // Function to call when the spinning has stopped.
        //'callbackSound'    : eachSegmentSpin,   // Called when the tick sound is to be played.
      }
};

const allOptions = {
  ...options,
  outerRadius: (canvasWidth / 2) - (options.lineWidth || 1)
};

onMounted(() => {
  allOptions.outerRadius = (canvasWidth / 2) - (options.lineWidth || 1);
});
</script>

<style scoped>
button{
  background-color: aqua;
  position: relative;
}
#canvas-container {
  display: flex !important;
  width: auto !important;
  height: auto !important;
  padding: 3rem !important;
  background-image: none;
}
#prize-pointer {
  transform: translateX(-50%);
  left: 50% !important
}

</style>