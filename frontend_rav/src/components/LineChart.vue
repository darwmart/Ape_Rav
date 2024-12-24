<template>
	<div>
		<Line ref="chartRef" :data="renderedChartData" :options="chartOptions" />
	</div>
</template>

<script setup>
import { Line } from "vue-chartjs";
import { Chart, registerables } from "chart.js";
import { reactive, onMounted, ref, computed } from "vue";
import axios from 'axios';

// Registrar los módulos necesarios de Chart.js
Chart.register(...registerables);
const host = import.meta.env.VITE_HOST;
const chartRef = ref(null);

const rawData = reactive({
	colocadosData: Array(12).fill(0),
	inscritosData: Array(12).fill(0),
});

// Datos calculados para pasar al gráfico
const renderedChartData = computed(() => ({
	labels: ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto", "Sept", "Nov", "Dic"],
	datasets: [
		{
			label: "Metas Colocados",
			borderColor: 'rgba(253, 195, 0, 1)',
			data: rawData.colocadosData,
			fill: false,
			tension: 0.1,
		},
		{
			label: "Metas Inscritos",
			borderColor: 'rgba(211, 26, 46, 1)',
			data: rawData.inscritosData,
			fill: false,
			tension: 0.1,
		},
	],
}));

const chartOptions = reactive({
	responsive: true,
	maintainAspectRatio: false,
	layout: {
		padding: {
			right: 50,
		},
	},
	scales: {
		x: {
			ticks: {
				color: 'white',
				font: {
					size: 14,
					weight: '600',
				},
			},
			title: {
				display: true,
				color: 'white',
				font: {
					size: 16,
					weight: '600',
				},
			},
			grid: {
				display: false,
			},
		},
		y: {
			ticks: {
				color: 'white',
				font: {
					size: 14,
					weight: '600',
				},
				stepSize: 5,
			},
			title: {
				display: true,
				color: 'white',
				font: {
					size: 16,
					weight: 'bold',
				},
			},
			grid: {
				display: false,
			},
			min: 0,
			max: 2,
		},
	},
	plugins: {
		legend: {
			labels: {
				color: 'white',
				font: {
					size: 14,
					weight: 'bold',
				},
			},
		},
	},
});

const fetchMensuales = async () => {
	try {
		const responseMes = await axios.get(`${host}:5010/api/v1/metas/ejecucionMensual`);
		const dataMes = responseMes.data;

		const monthIndex = (yearMonth) => parseInt(yearMonth.split('-')[1], 10) - 1;

		const colocadosData = Array(12).fill(0);
		const inscritosData = Array(12).fill(0);

		dataMes.colocados.forEach((item) => {
			const index = monthIndex(item.year_month);
			colocadosData[index] = item.count;
		});

		dataMes.inscritos.forEach((item) => {
			const index = monthIndex(item.year_month);
			inscritosData[index] = item.count;
		});

		// Actualizar datos reactivos
		rawData.colocadosData = colocadosData;
		rawData.inscritosData = inscritosData;
	} catch (error) {
		console.error('Error al obtener las metas mensuales:', error);
	}
};

onMounted(async () => {
	await fetchMensuales();
});
</script>

<style scoped>
/* Añade estilos personalizados aquí si los necesitas */
</style>
