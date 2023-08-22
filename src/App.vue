<template>
  <main>
    <div>
      <p>{{displayNumber}}</p>
      <button @click="generateNumber">Generate Number</button>
      <div>
        <label for="evens">Evens</label>
        <input v-model="evens" type="number" name="evens" id="evens">
        <button @click="evens=''">Clear</button>
      </div>
      
      <div>
        <label for="odds">Odds</label>
        <input v-model="odds" type="number" name="odds" id="odds">
        <button @click="odds=''" >Clear</button>
      </div>
    </div>

    <div id="results">
      <span v-for="number in results">{{ number }}</span>
    </div>
    
  
  </main>
</template>

<script setup>
import { ref } from 'vue';

const displayNumber = ref(0)
const odds = ref(0)
const evens = ref(0)
const results = ref([])
  

function generateNumber(){
  displayNumber.value = Math.floor(Math.random() * 38);
  results.value.unshift(displayNumber.value)
  console.log(results.value)
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
