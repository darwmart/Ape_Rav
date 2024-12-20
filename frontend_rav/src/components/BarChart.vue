<template>
	<div>
		<Bar ref="chartRef" :data="renderedChartData" :options="chartOptions" />
	</div>
</template>

<script setup>
import { Bar } from "vue-chartjs";
import { Chart, registerables } from "chart.js";
import { reactive, ref, computed, onMounted } from "vue";
import axios from "axios";

// Registrar los módulos necesarios de Chart.js
Chart.register(...registerables);

const chartRef = ref(null);

// Datos reactivos para manejar los valores crudos
const rawData = reactive({
	labels: [], // Etiquetas de las etnias
	data: [],   // Valores de los conteos
});

// Datos calculados para la gráfica
const renderedChartData = computed(() => ({
	labels: rawData.labels,
	datasets: [
		{
			label: "Colocados Por Tipo De Población",
			backgroundColor: "rgba(153, 102, 255, 0.2)",
			borderColor: "rgba(122, 31, 126)",
			data: rawData.data,
		},
	],
}));

const chartOptions = reactive({
	responsive: true,
	maintainAspectRatio: false,
});

// Función para obtener los datos desde la API
const fetchEtnias = async () => {
	try {
		const response = await axios.get("http://localhost:5010/api/v1/metas/ejecucionEtnia");
		const dataEtnia = response.data;

		// Extraer etiquetas y valores
		const labels = dataEtnia.map((item) => item.Etnia);
		const data = dataEtnia.map((item) => item.Count);

		// Actualizar los datos reactivos
		rawData.labels = labels;
		rawData.data = data;

	} catch (error) {
		console.error("Error al obtener las metas por etnia:", error);
	}
};

onMounted(async () => {
	await fetchEtnias();
});
</script>

<style scoped>
/* Añade estilos personalizados aquí si los necesitas */
</style>
