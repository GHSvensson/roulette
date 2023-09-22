<template>
<div class="tracker">
    <span v-for="(value, key) in streaks" :key="key">
      {{ key }}: {{ value }}
    </span>
  </div>
</template>

<script setup>
const props = defineProps({
    streaks:Object
});

</script>