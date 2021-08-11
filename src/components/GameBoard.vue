<template>
    <div class="board" :style="gridStyle">
        <GridCell
            v-for="(isAlive, index) in areAlive"
            :key="index"
            :index="index"
            :isAlive="isAlive"
            @toggled="toggleCell"
        />
    </div>
    <button @click="updateCells">Step</button>
    <button @click="randomize">Randomize</button>
</template>

<script lang="ts">
import GridCell from "./GridCell.vue";
import { defineComponent } from "vue";

export default defineComponent({
    name: "GameBoard",
    components: {
        GridCell,
    },
    props: {
        width: {
            type: Number,
            default: 16,
        },
        height: {
            type: Number,
            default: 16,
        },
        cellPx: {
            type: Number,
            default: 16,
        },
    },
    data() {
        const areAlive: boolean[] = Array(this.width * this.height).fill(false);
        return {
            areAlive,
        };
    },
    computed: {
        gridStyle() {
            return {
                "grid-template-columns": `repeat(${this.width}, 1fr)`,
                "grid-template-rows": `repeat(${this.height}, 1fr)`,
                width: this.width * this.cellPx + "px",
                height: this.height * this.cellPx + "px",
            };
        },
    },
    methods: {
        toggleCell(index: number) {
            this.areAlive[index] = !this.areAlive[index];
        },
        updateCells() {
            const newAlives = [];
            for (let x = 0; x < this.width; x++) {
                for (let y = 0; y < this.height; y++) {
                    newAlives[this.rawIndex(x, y)] = this.willLive(x, y);
                }
            }
            this.areAlive = newAlives;
        },
        randomize() {
            this.areAlive = this.areAlive.map(() => Math.random() < 0.5);
        },
        rawIndex(x: number, y: number) {
            return y * this.width + x;
        },
        isAlive(x: number, y: number) {
            return (
                x >= 0 &&
                x < this.width &&
                y >= 0 &&
                y < this.height &&
                this.areAlive[this.rawIndex(x, y)]
            );
        },
        willLive(x: number, y: number) {
            const neighbors = this.countNeighbors(x, y);
            return neighbors === 3 || (neighbors === 2 && this.isAlive(x, y));
        },
        countNeighbors(x: number, y: number) {
            let count = 0;
            for (let nX = x - 1; nX < x + 2; nX++) {
                for (let nY = y - 1; nY < y + 2; nY++) {
                    console.log(nX, nY);
                    if ((nX !== x || nY !== y) && this.isAlive(nX, nY)) {
                        count++;
                    }
                }
            }
            return count;
        },
    },
});
</script>

<style scoped>
.board {
    display: grid;
    border: 1px solid black;
}
</style>
