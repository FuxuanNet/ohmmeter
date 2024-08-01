<template>
    <div>
        <OhmmeterInput @add-entry="handleAddEntry" />
        <OhmmeterTable :entries="entries" />
        <OhmmeterDial :resistances="resistances" :ticks="ticks" />
    </div>
</template>

<script setup>
import { ref } from 'vue';
import OhmmeterInput from './OhmmeterInput.vue';
import OhmmeterTable from './OhmmeterTable.vue';
import OhmmeterDial from './OhmmeterDial.vue';

const entries = ref([
    { resistance: 0, tick: 25 },
    { resistance: 10, tick: 23 },
    // 初始化时可以设置一些默认值
]);

const resistances = ref(entries.value.map(entry => entry.resistance));
const ticks = ref(entries.value.map(entry => entry.tick));

const handleAddEntry = (entry) => {
    entries.value.push(entry);
    resistances.value.push(entry.resistance);
    ticks.value.push(entry.tick);
};
</script>
