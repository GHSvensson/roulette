<template>
  <main>
    <div id="autoSpin">
      <input v-model="autoSpinCounter">
      <button @click="autoSpin">Auto Spin</button>
    </div>
    <div>
      <Transition name="fade" mode="out-in">
        <p :key="displayNumber">{{displayNumber}}</p>
      </Transition>

      <div class="input-container">
        <label for="evens">Evens: {{ evensMulti }}x</label>
        <span>{{ evensWon ? '✅' : '❌' }}</span>
        <input v-model="evens" type="number" name="evens" id="evens">
        <button @click="evens=''">Clear</button>
      </div>

      <div class="input-container">
        <label for="odds">Odds: {{ oddsMulti }}x</label>
        <span>{{ oddsWon ? '✅' : '❌' }}</span>
        
        <input v-model="odds" type="number" name="odds" id="odds">
        <button @click="odds=''" >Clear</button>
      </div>
      <Transition name="fade" mode="out-in">
        <p :key="money">{{money}}:-</p>
      </Transition>
      <button class="bet-button" @click="generateNumber">BET</button>
    <input v-model="startingMoney">
    <button @click="money=startingMoney">New Balance</button>
    <div id="results">
      <div>
        <TransitionGroup name="results">
          <HistoryNumber v-for="number in results" :key="number.id" :number="number"></HistoryNumber>
        </TransitionGroup>
      </div>
      <button @click="results=[]">Clear</button>
    </div>
   <GameOver v-if="showGameOverModal" @hideModal="restartGame"></GameOver>
    </div>
    
   <div class="tracker">
   {{tenStreak}}
  </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import HistoryNumber from './components/HistoryNumber.vue';
import { computed } from 'vue';
import GameOver from './components/GameOver.vue';
import { v4 as uuidv4 } from 'uuid';


const displayNumber = ref(0)
const odds = ref(0)
const evens = ref(0)
const results = ref([])
const money = ref(100000)
const evensMulti = ref(1)
const oddsMulti = ref(1)
const startingMoney = ref()
const showGameOverModal = ref(false)
const consecutiveEvens = ref(0)
const consecutiveOdds = ref(0)
const tenStreak = ref(0)
const autoSpinCounter = ref(0)

const evensWon = computed(()=>{
  return (displayNumber.value % 2 === 0 && displayNumber.value !== 0)
})
const oddsWon = computed(()=>{
  return displayNumber.value % 2 !== 0;
})
const rolledZero = computed(()=>{
  return displayNumber.value === 0;
})
  
function generateNumber(){
  displayNumber.value = Math.floor(Math.random() * 38);
  results.value.unshift({
   number: displayNumber.value,
   id:uuidv4()
  })
  results.value = results.value.slice(0, 76)


  if (evensWon.value){
    money.value -= odds.value * oddsMulti.value
    money.value += evens.value * evensMulti.value
    oddsMulti.value *= 2
    evensMulti.value = 1

    consecutiveEvens.value++;
    consecutiveOdds.value = 0

    if (consecutiveEvens.value === 10){
      tenStreak.value++;
      consecutiveEvens.value = 0;
    }
  }

  if (oddsWon.value){
    money.value -= evens.value * evensMulti.value
    money.value += odds.value * oddsMulti.value
    evensMulti.value *= 2
    oddsMulti.value = 1

    consecutiveOdds.value++;
    consecutiveEvens.value = 0;

    if (consecutiveOdds.value === 10){
      tenStreak.value++;
      consecutiveOdds.value = 0;
  }}
    //multiplier applied to both in case of zero
  if (rolledZero.value){
    evensMulti.value *= 2
    oddsMulti.value *= 2
  }
  if (money.value<=0){
    showGameOverModal.value = true;
  }
}

function autoSpin(){
  for (let i = 0; i < autoSpinCounter.value; i++) {
  generateNumber();
}
}

function restartGame(){
  showGameOverModal.value = false;
   displayNumber.value = 0
   odds.value = 0
   evens.value = 0
   results.value = []
   money.value = 100000
   evensMulti.value = 1
   oddsMulti.value = 1
   startingMoney.value = 0
}

</script>

<style scoped>

p{
  font-size: 8rem;
  text-align: center;
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

main{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 5rem;
  height: 100%;
  place-content: center;
  max-width: 500px;
  margin-inline: auto;
  
}
main>div{
  margin-block: auto;
}
/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
.input-container{
  display: flex;
  justify-content: center;
}

label{
  display: block;
  text-align: center;
}

#autoSpin{
  
}

.tracker{
  width: 10rem;
  height: 10rem;
  background-color: white;
  font-size: 5rem;
  text-align: center;
}

.bet-button{
 width: auto;
 font-size: 2rem;
 margin-right: 2rem;
}

#results>div{
display: flex;
grid-gap: 1rem;
flex-wrap: wrap;
height: max-content;
margin: 1rem;
}
#results{
  display: grid;
}
  

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.results-move,
.results-enter-active,
.results-leave-active {
  transition: all 0.5s ease;
}
.results-enter-from,
.results-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

</style>
