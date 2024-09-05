<script setup lang="ts">
import { Chart, ArcElement, DoughnutController, Tooltip } from 'chart.js';
import { ref, onMounted, type PropType } from 'vue';

// Defining the props for the graph
const props = defineProps({
    doughnutBorderWidth: {
        type: Number,
        required: false,
        default: () => 10
    },
    doughnutBorderRadius: {
        type: Number,
        required: false,
        default: () => 15
    },
    doughnutCutout: {
        type: String,
        requred: false,
        default: () => "55%"
    },
    doughnutLabels: {
        type: Array as PropType<string[]>,
        required: false,
        default: () => ["test1", "test2"]
    },
    doughnutValues: {
        type: Array as PropType<Number[]>,
        required: false,
        default: () => [50, 50]
    },
    doughnutBackgroundColors: {
        type: Array as PropType<string[]>,
        required: false,
        default: () => ['#4CAF50', '#E8E8E8']
    },
    doughnutBorderColors: {
        type: Array as PropType<string[]>,
        required: false,
        default: () => ['#ffff']
    },
    doughnutCenterText: {
        type: String,
        required: false,
        default: () => ""
    },
    doughnutCenterTextColor: {
        type: String,
        required: false,
        default: () => "#000"
    },
    doughnutCenterTextFontSize: {
        type: Number,
        required: false,
        default: () => 30
    },
    doughnutCenterTextFontWeight: {
        type: String,
        required: false,
        default: () => 'bold'
    }
});

// Define the component
const myChart = ref<HTMLCanvasElement | null>(null);

onMounted(() => {
    if (!myChart.value) return;

    const ctx = myChart.value.getContext('2d');
    if (!ctx) return;

    // Register Chart.js elements
    Chart.register(ArcElement, DoughnutController, Tooltip);

    // Create the chart
    new Chart(ctx, {
        type: 'doughnut',
        data: {
            labels: props.doughnutLabels,
            datasets: [{
                label: 'Donut Chart',
                data: props.doughnutValues,
                backgroundColor: props.doughnutBackgroundColors,
                borderColor: props.doughnutBorderColors,
                hoverOffset: 4
            },]
        },
        options: {
            responsive: true,
            plugins: {
                legend: {
                    display: false, // Hide the legend
                },
                tooltip: {
                    callbacks: {
                        label: (context) => {
                            const labels = context.chart.data.labels;
                            if (labels) {
                                const label = labels[context.dataIndex];
                                const value = context.dataset.data[context.dataIndex];
                                return `    ${label} : ${value}%`;
                            }
                        }
                    }
                },
            },
            elements: {
                arc: {
                    borderWidth: props.doughnutBorderWidth,
                    borderRadius: props.doughnutBorderRadius,
                },
            },
            cutout: props.doughnutCutout,
        },
        plugins: [{
            id: 'centerText',
            afterDraw(chart) {
                const ctx = chart.ctx;
                ctx.save();
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.font = `${props.doughnutCenterTextFontWeight} ${props.doughnutCenterTextFontSize}px Arial`;
                ctx.fillStyle = props.doughnutCenterTextColor;
                ctx.fillText(props.doughnutCenterText, chart.width / 2, chart.height / 2);
                ctx.restore();
            }
        }]
    });
});
</script>

<template>
    <canvas ref="myChart"></canvas>
</template>