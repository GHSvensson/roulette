<template>
<Teleport to="body">
<div @click.self="$emit('hideModal')">
    <p>Your balance is less than 0:-</p>
</div>
</Teleport>
</template>

<script setup>
defineEmits(['hideModal'])
</script>

<style scoped>
div{
width: 100dvw;
height: 100dvh;
position: fixed;
display: flex;
top: 0;
left: 0;
}

p{
margin: auto;
font-size: 5rem;
background-color: black;
padding: 2rem;
border-radius: 3rem;

}

</style>

