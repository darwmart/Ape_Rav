<template>
	
	<div class=" bg-azulBarraApe  md:w-[260px] md:xl:w-72 md:rounded-[35px] md:mr-2 md:mt-2 md:h-[955px] ">
		<!-- Sidebar Derecho -->
		<div>
			<div class="flex flex-col items-center">
				<!-- Sección de Actividad -->
				<div
					class="bg-azulBarraApe md:w-full  flex justify-between px-4 items-center shadow-azulBarraApe md:shadow-none rounded-tr-[35px] rounded-tl-[35px] ">
					<h2 class="text-lg md:text-xl lg:text-[24px] lg:text-center font-bold text-amarillo mt-2 ">
						Actividad
					</h2>
					<div class="md:w-9 md:-mt-4 -mt-2 ">
						<router-link to="/IndicadoresActividad">
							<span>
								<svg class="md:w-10 md:h-10 h-6 w-6 " viewBox="0 0 30 35" fill="none"
									xmlns="http://www.w3.org/2000/svg">
									<rect x="0.5" y="-0.000244141" width="40.0001" height="40.0003" rx="20" />
									<path
										d="M23.95 11.7801C23.91 12.0301 23.89 12.2801 23.89 12.5301C23.89 14.7801 25.71 16.5991 27.95 16.5991C28.2 16.5991 28.44 16.5701 28.69 16.5301V24.5991C28.69 27.9901 26.69 30.0001 23.29 30.0001H15.901C12.5 30.0001 10.5 27.9901 10.5 24.5991V17.2001C10.5 13.8001 12.5 11.7801 15.901 11.7801H23.95ZM24.151 17.8601C23.88 17.8301 23.611 17.9501 23.45 18.1701L21.031 21.3001L18.26 19.1201C18.09 18.9901 17.89 18.9391 17.69 18.9601C17.491 18.9901 17.311 19.0991 17.19 19.2591L14.231 23.1101L14.17 23.2001C14 23.5191 14.08 23.9291 14.38 24.1501C14.52 24.2401 14.67 24.3001 14.84 24.3001C15.071 24.3101 15.29 24.1891 15.43 24.0001L17.94 20.7691L20.79 22.9101L20.88 22.9691C21.2 23.1391 21.6 23.0601 21.83 22.7591L24.72 19.0301L24.68 19.0501C24.84 18.8301 24.87 18.5501 24.76 18.3001C24.651 18.0501 24.41 17.8801 24.151 17.8601ZM28.0901 9.99976C29.4201 9.99976 30.5001 11.0798 30.5001 12.4098C30.5001 13.7398 29.4201 14.8198 28.0901 14.8198C26.7601 14.8198 25.6801 13.7398 25.6801 12.4098C25.6801 11.0798 26.7601 9.99976 28.0901 9.99976Z"
										fill="#FDC300" />
								</svg>
							</span>
						</router-link>
					</div>
				</div>
				<!--linea divisora text actividad-->
				<div class="bg-white md:w-[260px] md:h-0.5 -mt-4 "></div>
				<div class="md:grid md:grid-cols-1 bg-azulBarraApe  md:gap-5 gap-2 md:p-3 flex flex-col p-2 flex-grow">
					<!-- Agrega un gap para separación -->
					<div v-for="(goal, index) in goals" :key="index"
						class="indicator-wrapper flex items-center  overflow-hidden md:rounded-[35px] rounded-lg bg-"
						:style="getGradientStyle(index)">


						<!-- Texto a la izquierda del indicador -->
						<div class="grid grid-cols-1 justify-center items-start w-full md:h-56  md:mx-4 md:mt-5 ">
							<div :class="index === 2 ? 'text-white' : 'text-white'"
								class="md:text-[14px] font-base md:-mb-2 text-[13px]">
								{{ goal.label }}
							</div>
							<div :class="index === 2 ? 'text-white md:text-2xl font-bold' : 'text-white md:text-[24px] font-bold'"
								class="md:-mb-4">
								{{ goal.meta || 0 }}
							</div>
							<!--linea divisora texto meta -->
							<div class="bg-amarillo md:w-[100px] h-[1px] w-[80px] "></div>

							<div :class="index === 2 ? 'text-white' : 'text-white'"
								class="md:-mt-3 md:text-[14px] font-base text-[11px]">
								Estado Actual
							</div>
							<div :class="index === 2 ? 'text-white md:text-2xl font-bold' : 'text-white md:text-[24px] font-bold'"
								class="md:-mt-4 ">
								{{ goal.current || 0 }}
							</div>
							<div class="flex flex-col md:text-[12px] text-[10px] my-7">
								<!-- Círculo blanco con texto -->
								<div class="flex items-center md:mb-2">
									<div class="md:w-5 md:h-5 w-3 h-3 rounded-full bg-white border border-gray-400"></div>
									<span class="md:ml-3 text-white">Carga</span>
								</div>

								<!-- Círculo amarillo con texto -->
								<div class="flex items-center">
									<div class="md:w-5 md:h-5 rounded-full bg-amarillo w-3 h-3"></div>
									<span class="md:ml-3 text-white">Cargado</span>
								</div>
							</div>

						</div>
						<!-- Indicador Circular -->
						<div class="relative w-32 h-32 md:w-[240px] md:h-[240px] md:-mb-20  -mb-4">
							<svg class="w-full h-full" viewBox="18 -10 75 100">
								<circle cx="50" cy="50" r="30" stroke="lightgray" stroke-width="7" fill="none" />
								<circle cx="50" cy="50" r="30" stroke="currentColor" :stroke-dasharray="circumference"
									:stroke-dashoffset="circumference - (circumference * goal.value) / 100"
									stroke-width="7" fill="none"
									class="text-amarillo transition-all duration-1000 ease-out" />
							</svg>
							<span
								class="absolute md:inset-5 inset-4 h-[115px] flex items-center  md:text-2xl text-lg font-bold  md:h-56  text-center text-white ">
								{{ goal.value }}%
							</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import axios from 'axios';
// Estado para controlar la visibilidad
const isVisible = ref(true);

// Función para alternar visibilidad
const toggleVisibility = () => {
  isVisible.value = !isVisible.value;
};
// Estado de las metas (si es necesario para otras funcionalidades)180deg, 
const host = import.meta.env.VITE_HOST;
const getGradientStyle = () => {
	return {
		background: 'linear-gradient(180deg, rgba(0, 0, 0, 0.05) 0%, rgba(0, 0, 0, 0.3) 90%)',
		backgroundBlendMode: '',
	};
};

const goals = ref([
	{ label: "Meta Anual", value: 100, meta: 1000, current: 190 },
	{ label: "Meta Trimestral", value: 100, meta: 333, current: 50 },
	{ label: "Meta Mensual", value: 100, meta: 30, current: 15 }
]);

// Duncion para obtener la informacion de las metas
const fetchMetasData = async () => {
	try {
		const responseMetas = await axios.get(`${host}:8084/api/v1/metas`);
		const dataMetas = responseMetas.data;
		goals.value[0].meta = dataMetas.data[0].cifra_meta;
		goals.value[1].meta = dataMetas.data[1].cifra_meta;
		goals.value[2].meta = dataMetas.data[2].cifra_meta;



	} catch (error) {
		console.error('Error al obtener las metas anuales:', error);
	}
};


// Función para obtener los totales desde la API
const fetchTotales = async () => {
	try {
		const responseAnual = await axios.get(`${host}:8084/api/v1/metas/ejecuciónAnual`);
		const dataAnual = responseAnual.data;

		goals.value[0].current = dataAnual.total_orientados;


	} catch (error) {
		console.error('Error al obtener las metas anuales:', error);
	}
};

const fetchTrimestral = async () => {
	try {

		const responseTrimestral = await axios.get(`${host}:8084/api/v1/metas/ejecucionTrimestral`);
		const dataTrimestral = responseTrimestral.data;

		goals.value[1].current = dataTrimestral[0].count;

	} catch (error) {
		console.error('Error al obtener las metas trimestrales:', error);
	}
};

const fetchMensuales = async () => {
	try {
		const responseMes = await axios.get(`${host}:8084/api/v1/metas/ejecucionMensualNotificacion`);
		const dataMes = responseMes.data;

		goals.value[2].current = dataMes.count;

	} catch (error) {
		console.error('Error al obtener las metas mensuales:', error);
	}
};

const circumference = 2 * Math.PI * 40;
// Simular valores de las metas (puedes eliminar esta parte si no es necesario)
onMounted(() => {
	fetchMetasData();
	fetchTotales();
	fetchTrimestral();
	fetchMensuales();

	setTimeout(() => {

		goals.value[0].value = ((goals.value[0].current / goals.value[0].meta) * 100).toFixed(0);
		goals.value[1].value = ((goals.value[1].current / goals.value[1].meta) * 100).toFixed(0);
		goals.value[2].value = ((goals.value[2].current / goals.value[2].meta) * 100).toFixed(0);

	}, 500);
});

</script>
<style scoped>
/* Rotación de la flecha con transición */
.rotate-180 {
  transform: rotate(180deg);
  transition: transform 0.3s ease-in-out;
}</style>