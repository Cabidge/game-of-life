<template>
    <div
        @mousedown="onMouseEvent"
        @mouseenter="onMouseEvent"
        :class="{ alive: isAlive }"
        draggable="false"
    ></div>
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
    methods: {
        onMouseEvent(e: MouseEvent) {
            if (e.buttons == 1) {
                this.$emit("toggled", this.index);
            }
        },
    },
    emits: ["toggled"],
});
</script>

<style scoped>
div {
    background-color: white;
    border: 1px solid rgba(0, 0, 0, 0.1);
}

div.alive {
    background-color: black;
}
</style>
