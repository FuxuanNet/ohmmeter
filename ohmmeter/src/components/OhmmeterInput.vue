<template>
    <div>
        <h3>输入电阻和刻度</h3>
        <form @submit.prevent="addEntry">
            <div>
                <label for="resistance">电阻 (Ω):</label>
                <input type="number" v-model="newResistance" step="0.01" required />
            </div>
            <div>
                <label for="tick">刻度 (格):</label>
                <input type="number" v-model="newTick" step="0.01" required />
            </div>
            <button type="submit">添加</button>
        </form>
    </div>
</template>

<script setup>
import { ref, emit } from 'vue';

const newResistance = ref('');
const newTick = ref('');

const addEntry = () => {
    const resistanceValue = parseFloat(newResistance.value);
    const tickValue = parseFloat(newTick.value);

    if (!isNaN(resistanceValue) && !isNaN(tickValue)) {
        emit('add-entry', { resistance: resistanceValue, tick: tickValue });
        newResistance.value = '';
        newTick.value = '';
    }
};
</script>
