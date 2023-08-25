<template>
  <main>
    <div>
      <Transition name="fade" mode="out-in">
        <p :key="displayNumber">{{displayNumber}}</p>
      </Transition>
      
      <button @click="generateNumber">BET</button>
      <div>
        <label for="evens">Evens: {{ evensMulti }}x</label>
        <span>{{ evensWon ? '✅' : '❌' }}</span>
        <input v-model="evens" type="number" name="evens" id="evens">
        <button @click="evens=''">Clear</button>
      </div>
      
      <div>
        <label for="odds">Odds: {{ oddsMulti }}x</label>
        <span>{{ oddsWon ? '✅' : '❌' }}</span>
        
        <input v-model="odds" type="number" name="odds" id="odds">
        <button @click="odds=''" >Clear</button>
      </div>
      <Transition name="fade" mode="out-in">
        <p :key="money">{{money}}:-</p>
      </Transition>
    </div>
    
    <div id="results">
     <TransitionGroup name="results">
      <HistoryNumber v-for="number in results" :key="number" :number="number"></HistoryNumber>
     </TransitionGroup>
      <button @click="results=[]">Clear</button>
    </div>
  
  </main>
</template>

<script setup>
import { ref } from 'vue';
import HistoryNumber from './components/HistoryNumber.vue';
import { computed } from 'vue';


const displayNumber = ref(0)
const odds = ref(0)
const evens = ref(0)
const results = ref([])
const money = ref(100000)
const evensMulti = ref(1)
const oddsMulti = ref(1)

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
  results.value.unshift(displayNumber.value)


  if (evensWon.value){
    money.value -= odds.value
    oddsMulti.value *= 2
    money.value += evens.value * evensMulti.value
    evensMulti.value = 1
  }

  if (oddsWon.value){
    money.value -= evens.value
    evensMulti.value *= 2
    money.value += odds.value * oddsMulti.value
    oddsMulti.value = 1
  }

  if (rolledZero.value){
    evensMulti.value = 1
    oddsMulti.value = 1
  }
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
button{
  margin-inline: auto;
}
main{
  display: grid;
  grid-template-columns: 1fr 1fr;
  height: 100%;
  
}
main>div{
  margin-block: auto;
}
/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

label{
  display: block;
  text-align: center;
}
span{

}
#results{
display: flex;
grid-gap: 1rem;
flex-wrap: wrap;
height: max-content;
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
