<template>
    <div class="board" :style="gridStyle">
        <GridCell
            v-for="(n, index) in width * height"
            :key="n"
            :index="index"
            :isAlive="areAlive.has(index)"
            @toggled="toggleCell"
        />
    </div>
    <label for="auto-step">Auto-Step</label>
    <input
        id="auto-step"
        type="checkbox"
        @change="updateAuto"
        v-model="autoEnabled"
    />
    <button @click="updateCells">Step</button>
    <button @click="randomize">Randomize</button>
    <button @click="clear">Clear</button>
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
        return {
            areAlive: new Set<number>(),
            autoIntervalId: 0,
            autoEnabled: false,
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
            if (!this.areAlive.delete(index)) {
                this.areAlive.add(index);
            }
        },
        updateAuto() {
            if (this.autoEnabled) {
                this.updateCells();
                this.autoIntervalId = setInterval(this.updateCells, 100);
            } else {
                clearInterval(this.autoIntervalId);
            }
        },
        updateCells() {
            const neighborCount = new Map<number, number>();
            this.areAlive.forEach((index) => {
                const [x, y] = this.splitXY(index);
                for (let x2 = x - 1; x2 < x + 2; x2++) {
                    for (let y2 = y - 1; y2 < y + 2; y2++) {
                        console.log(x2, y2);
                        if (
                            x2 >= 0 &&
                            x2 < this.width &&
                            y2 >= 0 &&
                            y2 < this.height &&
                            (x2 != x || y2 != y)
                        ) {
                            const index2 = this.rawIndex(x2, y2);
                            const newCount =
                                (neighborCount.get(index2) || 0) + 1;
                            neighborCount.set(index2, newCount);
                        }
                    }
                }
            });
            const newAlives = new Set<number>();
            neighborCount.forEach((count, index) => {
                if (count == 3 || (count == 2 && this.areAlive.has(index))) {
                    newAlives.add(index);
                }
            });
            this.areAlive = newAlives;
        },
        splitXY(index: number) {
            const x = index % this.width;
            const y = ~~(index / this.width);
            return [x, y];
        },
        clear() {
            this.areAlive = new Set();
        },
        randomize() {
            this.clear();
            for (let i = 0; i < this.width * this.height; i++) {
                if (Math.random() < 0.5) {
                    this.areAlive.add(i);
                }
            }
        },
        rawIndex(x: number, y: number) {
            return y * this.width + x;
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
