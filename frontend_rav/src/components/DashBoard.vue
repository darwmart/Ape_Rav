<template>
	<div class="md:flex md:flex-col md:gap-5 md:p-10 grid grid-cols-1 gap-3 p-">
		<!-- Primera Sección: Contenedores con contenido e imagen -->
		<div class="md:flex md:gap-5 md:justify-between grid grid-cols-1 gap-1 md:justify-items-center ">
			<div
				v-for="(image, index) in imagePaths"
				:key="index"
				class="md:w-[360px] md:h-[235px] w-[359px] h-[159px] my-card md:bg-center md:rounded-[10px] bg-blend-multiply relative md:bg-inherit  bg-top"
				:style="{
					backgroundImage:
						'url(' +
						image.path +
						'), linear-gradient(180deg, rgba(0, 0, 0, 0) 16%, #315CA0 85.5%)',
					
				}">
				<!-- Contenedor para el título y el número en la parte inferior -->
				<div
					class="absolute bottom-0 left-0 w-full bg-gradient-to-t from-black via-transparent to-transparent rounded-lg">
					<h4 class="md:font-bold text-white md:text-3xl text-center">
						{{ image.title }}
					</h4>
					<div class="md:text-3xl font-semibold text-white text-center md:-mt-10 -mt-5">
						<p>{{ image.count }}</p>
					</div>
				</div>
			</div>
		</div>

		<!-- Segunda Sección: Gráficos -->
		<div class="md:grid md:gap-5 md:grid-cols-1 lg:grid-cols-1 grid grid-cols-1 gap-3 justify-items-center">
			<div
				class="md:border my-card rounded-lg bg-custom-gradient w-[358px]  md:w-full md:h-72 md:p-16 ">
				<LineChart />
			</div>
			<div
				class="md:p-20 border my-card border-red-500 rounded-lg bg-white w-[358px]  md:w-full md:h-72 ">
				<BarChart />
			</div>
		</div>

		
	</div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import LineChart from "@/components/LineChart.vue";
import BarChart from "@/components/BarChart.vue";
import EnFormacion from "@/assets/images/Orientados.svg";
import Certificados from "@/assets/images/Colocados.svg";
import EnProceso from "@/assets/images/Inscritos.svg";
import axios from 'axios';

// Datos de las imágenes estáticas y sus títulos
const imagePaths = ref([
	{
		title: "ORIENTADOS",
		count: "12883124",
		path: EnFormacion,
	},
	{
		title: "COLOCADOS",
		count: "9888112",
		path: Certificados,
	},
	{
		title: "INSCRITOS",
		count: "85883161",
		path: EnProceso,
	},

]);
const host = import.meta.env.VITE_HOST;

// Función para obtener los totales desde la API
const fetchTotales = async () => {
  try {
    const response = await axios.get(`${host}:8084/api/v1/metas/ejecuciónAnual`);
    const data = response.data;

    // Actualizamos los counts en la lista imagePaths
    imagePaths.value[0].count = data.total_orientados;
    imagePaths.value[1].count = data.total_colocados;
    imagePaths.value[2].count = data.total_inscritos;
  } catch (error) {
    console.error('Error al obtener los totales:', error);
  }
};

onMounted(() => {
	fetchTotales();
});
</script>

<style scoped>
@import "@/styles/table-styles.css";
.my-card {
	transition: transform 0.3s ease-in-out; /* Transición de la propiedad transform con duración de 300ms */
	box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.my-card:hover {
	transform: scale(1.05);
}

</style>
