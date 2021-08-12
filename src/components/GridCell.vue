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
    background-color: white;
    border: 1px solid rgb(235, 235, 235);
}

div.alive {
    background-color: black;
    border: 1px solid black;
}
</style>
