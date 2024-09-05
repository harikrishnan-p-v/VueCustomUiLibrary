<script setup lang="ts">
import { Chart, CategoryScale, BarController, LinearScale, BarElement, Title, Tooltip, Legend } from 'chart.js';
import { ref, computed, onMounted, type PropType } from 'vue';

interface dataSet {
    label: string,
    data: number[],
    backgroundColor: string,
    barThickness: number,
    borderRadius: number,
}

// Defining the props for the graph
const props = defineProps({
    barXaxisLables: {
        type: Array as PropType<string[]>,
        require: false,
        default: () => ["test1", "test2"]
    },
    barDataSets: {
        type: Array as PropType<dataSet[]>,
        require: false,
        default: () => [
            {
                label: 'testData1',
                data: [20, 10],
                backgroundColor: '#C2B8EE',
                barThickness: 25,
                borderRadius: 6,
            },
            {
                label: 'testData2',
                data: [40, 10],
                backgroundColor: '#8C7BD6',
                barThickness: 25,
                borderRadius: 6
            },
        ]
    },
    xAxisTitle: {
        type: String,
        required: false,
        default: ""
    },
    yAxisTitle: {
        type: String,
        required: false,
        default: ""
    },
    xAxisTitleFontSize: {
        type: Number,
        required: false,
        default: 12
    },
    yAxisTitleFontSize: {
        type: Number,
        required: false,
        default: 12
    },
    yAxisStepSize: {
        type: Number,
        required: false,
        default: 5
    }
});

const barHighestValue = computed(() => {
    return Math.ceil(Math.max(...props.barDataSets.flatMap((dataSet) => dataSet.data)) / 10) * 10;
})

// Define the component
const myChart = ref<HTMLCanvasElement | null>(null);

onMounted(() => {
    if (!myChart.value) return;

    const ctx = myChart.value.getContext('2d');
    if (!ctx) return;

    // Register Chart.js elements
    Chart.register(CategoryScale, BarController, LinearScale, BarElement, Title, Tooltip, Legend);

    // Create the chart
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: props.barXaxisLables,
            datasets: props.barDataSets
        },
        options: {
            responsive: true,
            scales: {
                x: {
                    title: {
                        display: true,
                        text: props.xAxisTitle,
                        font: {
                            size: props.xAxisTitleFontSize,
                            weight: "bold"
                        }
                    },
                    beginAtZero: true,
                    stacked: false, // Ensure bars are side by side
                    grid: {
                        display: false, // Remove grid lines
                    },
                },
                y: {
                    title: {
                        display: true,
                        text: props.yAxisTitle,
                        font: {
                            size: props.yAxisTitleFontSize,
                            weight: "bold",
                        }
                    },
                    beginAtZero: true,
                    max: barHighestValue.value,
                    ticks: {
                        stepSize: props.yAxisStepSize
                    },
                    grid: {
                        display: false, // Remove grid lines
                    },
                },
            },
            plugins: {
                legend: {
                    position: 'top',
                },
            },
        },
    });
});
</script>

<template>
    <canvas ref="myChart"></canvas>
</template>