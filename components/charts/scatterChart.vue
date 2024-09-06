<script setup lang="ts">
import { Chart, ScatterController, PointElement, LinearScale, Title, Tooltip } from 'chart.js';
import { ref, onMounted, type PropType } from 'vue';

interface data {
    x: number,
    y: number,
    description: string
}

interface dataSet {
    label: string,
    data: data[],
    backgroundColor: string,
    pointRadius: number,
    pointHoverRadius: number,
}

// Defining the props for the graph
const props = defineProps({
    scatterDataSets: {
        type: Array as PropType<dataSet[]>,
        require: false,
        default: () => [
            {
                label: 'testData1',
                data: [{
                    x: 8,
                    y: 4,
                    description: 'test1'
                }],
                backgroundColor: '#C2B8EE',
                pointRadius: 25,
                pointHoverRadius: 26,
            },
            {
                label: 'testData2',
                data: [{
                    x: 5,
                    y: 6,
                    description: 'test2'
                }],
                backgroundColor: '#8C7BD6',
                pointRadius: 5,
                pointHoverRadius: 6,
            },
        ]
    },
    xAxisTitle: {
        type: String,
        required: false,
        default: () => "x-axis"
    },
    yAxisTitle: {
        type: String,
        required: false,
        default: () => "y-axis"
    },
    xAxisTitleFontSize: {
        type: Number,
        required: false,
        default: () => 12
    },
    yAxisTitleFontSize: {
        type: Number,
        required: false,
        default: () => 12
    },
    xAxisStepSize: {
        type: Number,
        required: false,
        default: () => 2
    },
    yAxisStepSize: {
        type: Number,
        required: false,
        default: () => 2
    }
});

// Define the component
const myChart = ref<HTMLCanvasElement | null>(null);

onMounted(() => {
    if (!myChart.value) return;

    const ctx = myChart.value.getContext('2d');
    if (!ctx) return;

    // Register Chart.js elements
    Chart.register(ScatterController, PointElement, LinearScale, Title, Tooltip);

    // Create the chart
    new Chart(ctx, {
        type: 'scatter',
        data: {
            datasets: props.scatterDataSets
        },
        options: {
            responsive: true,
            scales: {
                x: {
                    type: 'linear',
                    title: {
                        display: true,
                        text: props.xAxisTitle,
                        font: {
                            size: props.xAxisTitleFontSize,
                            weight: "bold"
                        }
                    },
                    min: 0,
                    max: 10,
                    ticks: {
                        callback: function () {
                            return ''; // Remove numbers from x axis
                        },
                        stepSize: props.xAxisStepSize
                    },
                },
                y: {
                    type: 'linear',
                    title: {
                        display: true,
                        text: props.yAxisTitle,
                        font: {
                            size: props.yAxisTitleFontSize,
                            weight: "bold"
                        }
                    },
                    min: 0,
                    max: 10,
                    ticks: {
                        callback: function () {
                            return ''; // Remove numbers from x axis
                        },
                        stepSize: props.yAxisStepSize
                    },
                },
            },
            plugins: {
                legend: {
                    position: 'top',
                    display: true
                },
                tooltip: {
                    callbacks: {
                        label: function (context) {
                            const point = context.raw as data;
                            return point.description;
                        },
                    },
                },
            },
        },
    });
});
</script>

<template>
    <canvas ref="myChart"></canvas>
</template>