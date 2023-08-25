<template>
  <main>
    <div>
      <p>{{displayNumber}}</p>
      <button @click="generateNumber">BET</button>
      <div>
        <label for="evens">Evens</label>
        <span>{{ evensWon && !rolledZero ? '✅' : '❌' }}</span>
        <input v-model="evens" type="number" name="evens" id="evens">
        <button @click="evens=''">Clear</button>
      </div>
      
      <div>
        <label for="odds">Odds</label>
        <span>{{ oddsWon ? '✅' : '❌' }}</span>
        
        <input v-model="odds" type="number" name="odds" id="odds">
        <button @click="odds=''" >Clear</button>
      </div>
        <p>{{money}}:-</p>
    </div>
    

    <div id="results">
      <HistoryNumber v-for="number in results" :key="number" :number="number"></HistoryNumber>
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

const evensWon = computed(()=>{
return displayNumber.value % 2 === 0;
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

</style>
