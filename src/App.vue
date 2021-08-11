<template>
    <GameBoard />
</template>

<script lang="ts">
import GameBoard from "./components/GameBoard.vue";
import { defineComponent } from "vue";

export default defineComponent({
    name: "App",
    components: {
        GameBoard,
    },
});
</script>

<style></style>
