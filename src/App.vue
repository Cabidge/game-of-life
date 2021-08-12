<template>
    <input type="number" v-model="width" />
    <input type="number" v-model="height" />
    <input type="number" v-model="cellPx" />
    <GameBoard :width="width" :height="height" :cellPx="cellPx" />
</template>

<script lang="ts">
import GameBoard from "./components/GameBoard.vue";
import { defineComponent } from "vue";

export default defineComponent({
    name: "App",
    components: {
        GameBoard,
    },
    data() {
        return {
            width: 64,
            height: 32,
            cellPx: 16,
        };
    },
});
</script>

<style></style>
