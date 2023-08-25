<template>
<span :style="[ isEven ? 'color:red' : 'color:green']">{{ number.number }}</span>

</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
    number: Object
})

const isEven = computed(()=>{
    return (props.number.number % 2 === 0);
})
</script>