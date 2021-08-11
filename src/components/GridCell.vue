<template>
    <div @click="$emit('toggled', index)" :class="{ alive: isAlive }"></div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    name: "GridCell",
    props: {
        index: {
            type: Number,
            required: true,
        },
        isAlive: {
            type: Boolean,
            required: true,
        },
    },
    emits: ["toggled"],
});
</script>

<style scoped>
div {
    background-color: black;
}

div.alive {
    background-color: white;
}
</style>
