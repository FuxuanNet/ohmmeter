<template>

    <div class="ohmmeter-dial">
        <canvas ref="dialCanvas" :width="canvasWidth" :height="canvasHeight"></canvas>
    </div>

</template>

<script setup>
import { onMounted, onUpdated, ref } from 'vue';

const props = defineProps({
    resistances: {
        type: Array,
        default: () => [0, 10, 20, 30, 60, 90, 120, 280, 360, 480, 600, 1000, 2000, 3000, '∞'],
    },
    ticks: {
        type: Array,
        default: () => [25, 23, 22, 21, 18, 15, 13, 9, 8, 6, 5, 3, 2, 1, 0],
    },
});

const canvasWidth = ref(800);
const canvasHeight = ref(800);
const dialCanvas = ref(null);

onMounted(() => {
    drawDial(props);
});

onUpdated(() => {
    drawDial(props);
});

function drawDial(props) {
    const canvas = dialCanvas.value;
    const ctx = canvas.getContext("2d");

    // 清空画布
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const centerX = canvasWidth.value / 2;
    const centerY = canvasHeight.value / 2;
    const radius = Math.min(centerX, centerY) * 0.9;

    const angles = props.ticks.map((tick) => (tick / 25) * (2 / 3) * Math.PI - (Math.PI * 5 / 6));

    angles.forEach((angle, index) => {
        const tick = props.ticks[index];
        const resistance = props.resistances[index];
        const startX = centerX + radius * Math.cos(angle);
        const startY = centerY + radius * Math.sin(angle);
        const endX = centerX + (radius - 30) * Math.cos(angle);
        const endY = centerY + (radius - 30) * Math.sin(angle);

        ctx.beginPath();
        ctx.moveTo(startX, startY);
        ctx.lineTo(endX, endY);
        ctx.strokeStyle = "red";
        ctx.lineWidth = 2;
        ctx.stroke();

        const labelX = centerX + (radius - 45) * Math.cos(angle);
        const labelY = centerY + (radius - 45) * Math.sin(angle);
        ctx.font = "14px Arial";
        ctx.fillStyle = "black";
        ctx.textAlign = angle < -Math.PI / 3 ? "left" : angle > -2 * Math.PI / 3 ? "right" : "center";
        ctx.fillText(`${resistance}Ω`, labelX, labelY);
    });

    for (let i = 0; i <= 100; i++) {
        const angle = (i / 100) * (2 / 3) * Math.PI - (Math.PI * 5 / 6);
        const startX = centerX + radius * Math.cos(angle);
        const startY = centerY + radius * Math.sin(angle);
        const endX = centerX + (radius - (i % 10 === 0 ? 15 : 10)) * Math.cos(angle);
        const endY = centerY + (radius - (i % 10 === 0 ? 15 : 10)) * Math.sin(angle);

        ctx.beginPath();
        ctx.moveTo(startX, startY);
        ctx.lineTo(endX, endY);
        ctx.strokeStyle = "gray";
        ctx.lineWidth = 1;
        ctx.stroke();

        if (i % 10 === 0) {
            const labelX = centerX + (radius - 30) * Math.cos(angle);
            const labelY = centerY + (radius - 30) * Math.sin(angle);
            ctx.font = "12px Arial";
            ctx.fillStyle = "gray";
            ctx.textAlign = "center";
            ctx.fillText(`${i}uA`, labelX, labelY);

            if (i === 50) {
                ctx.font = "14px Arial";
                ctx.fillText("R中", labelX, labelY - 35);
            }
        }
    }
}
</script>

<style scoped>
.ohmmeter-dial {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}
</style>
