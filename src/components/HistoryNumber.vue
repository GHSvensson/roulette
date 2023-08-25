<template>
<span :style="[ isEven ? 'color:red' : 'color:green']">{{ number }}</span>

</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
    number: Number 
})

const isEven = computed(()=>{
    return (props.number % 2 === 0);
})
</script>