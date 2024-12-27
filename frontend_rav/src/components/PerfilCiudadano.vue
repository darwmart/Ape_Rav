<template>
	<div class="min-h-screen md:p-4 flex flex-col  md:-mt-20 -mt-10 ">
		<!-- Sección superior: Título e Icono -->
		<div class="flex flex-col md:flex-row items-center justify-between md:p-2 rounded-lg md:max-w-7xl">
			<div class="flex items-center mb-3 md:mb-4 ">
				<div class="md:p-6 bg-azulBarraApe p-4 rounded-full">
					<!-- Icono SVG -->
					<img :src="Ciudadano" alt="Icono de Ciudadano" class="md:w-16 md:h-16 h-8 w-9" />
				</div>
				<div class="md:ml-4 ml-3 text-left md:mt-14">
					<p class="text-black -mb-1 text-2xl md:text-[37px]">Perfil del</p>
					<h2 class="text-customPurple text-5xl md:text-[94px] mt-0 font-bold">Ciudadano</h2>
				</div>
			</div>
			<!-- Botones de Acción 
			<div class="flex flex-wrap gap-4 mt-4 justify-center">-->
			<!--<router-link
					:to="{ name: 'RutaAtencionPage' }"
					class="cursor-pointer bg-customPurple border-none text-white py-4 px-6 rounded-lg flex flex-col items-center justify-center">
					<img
						:src="RutaAtencion"
						alt="Icono de Atención"
						class="w-10 h-10 mb-2" />
					<span class="text-sm">Ver Ruta de Atención</span>
				</router-link>-->

			<!--<router-link
					:to="{ name: 'LineaTiempoNuevaPage' }"
					class="bg-customPurple border-none text-white py-4 px-6 rounded-lg flex flex-col items-center justify-center">
					<img :src="VerLine" alt="Icono de Ver Línea" class="w-10 h-10 mb-2" />
					<span class="text-sm">Ver Línea de Tiempo</span>
				</router-link>-->

			<!--<router-link
					:to="{ name: 'RegistroActividadPage' }"
					class="bg-customPurple border-none text-white py-4 px-6 rounded-lg flex flex-col items-center justify-center">
					<img
						:src="Actividad"
						alt="Icono registrar actividad"
						class="w-10 h-10 mb-2" />
					<span class="text-sm">Registrar de Actividad</span>
				</router-link>
			</div>-->
		</div>

		<!-- Información personal del ciudadano -->
		<div class="flex flex-col lg:flex-row bg-white p-4 rounded-lg gap-8 mb-8 shadow-md ">
			<!-- Primera columna -->
			<div class="flex-1">
				<div class="flex items-center">
					<label class="text-black font-semibold w-1/3  md:text-lg text-xs">TIPO DE DOCUMENTO:</label>
					<p class="text-black w-2/3 md:text-lg text-xs">
						{{ data.tipo_identificacion || "NO REGISTRA" }}
					</p>
				</div>
				<div class="flex-1">
					<div class="flex items-center">
						<label class="text-black font-semibold w-1/3 md:text-lg text-xs">NÚMERO DE DOCUMENTO:</label>
						<p class="text-black w-2/3 md:text-lg text-xs">
							{{ data.identificacion || "NO REGISTRA" }}
						</p>
					</div>

					<div class="flex items-center">
						<label class="text-black font-semibold w-1/3 md:text-lg text-xs">NOMBRES:</label>
						<p class="text-black w-2/3 md:text-lg text-xs">
							{{ data.nombres || "NO REGISTRA" }}
						</p>
					</div>
					<div class="flex items-center">
						<label class="text-black font-semibold w-1/3 md:text-lg text-xs">APELLIDOS:</label>
						<p class="text-black w-2/3 md:text-lg text-xs">
							{{ data.apellidos || "NO REGISTRA" }}
						</p>
					</div>

				</div>
				<div class="flex items-center">
					<label class="text-black font-semibold w-1/3 md:text-lg text-xs">PROCEDENCIA ÉTNICA:</label>
					<p class="text-black w-2/3 md:text-lg text-xs">
						{{ data.etnia || "NO REGISTRA" }}
					</p>
				</div>

				<div class="flex items-center">
					<label class="text-black font-semibold w-1/3 md:text-lg text-xs">ESTADO VICTIMA:</label>
					<p class="text-black w-2/3 md:text-lg text-xs">
						{{ data.message || "NO REGISTRA" }}
					</p>
				</div>

				<div class="flex items-center">
					<label class="text-black font-semibold w-1/3 md:text-lg text-xs">HECHOS:</label>
					<p class="text-black w-2/3 md:text-lg text-xs" v-for="(hecho, index) in data.caracterizacion"
						:key="index">{{ hecho || "NO REGISTRA" }}</p>
				</div>


			</div>
		</div>
		<!-- Historial del Ciudadano 
		<div class="text-white rounded-lg shadow-md">-->
		<!-- Título del historial 
			<div class="flex items-center bg-customPurple p-px">
				<img :src="Historial" alt="Icono de Historial" class="w-8 h-8 mr-2" />
				<h3 class="text-xl font-bold">HISTORIAL DEL CIUDADANO</h3>
			</div>-->
		<!-- Contenedor de la tabla con scroll horizontal 
			<div class="overflow-x-auto">
				<table class="min-w-full bg-white text-gray-900">
					<thead>
						<tr class="bg-gray-200">
							<th class="px-4 py-2 border">FICHA</th>
							<th class="px-4 py-2 border">CURSO</th>
							<th class="px-4 py-2 border">ESTADO</th>
							<th class="px-4 py-2 border">TIPO</th>
							<th class="px-4 py-2 border">MODALIDAD</th>
							<th class="px-4 py-2 border">FECHA INICIO</th>
							<th class="px-4 py-2 border">FECHA FIN</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(course, index) in fetchData" :key="index">
							<td class="px-4 py-2 border">{{ course.FIC_ID }}</td>
							<td class="px-4 py-2 border">{{ course.PRF_DENOMINACION }}</td>
							<td class="px-4 py-2 border">{{ course.RGA_ESTADO }}</td>
							<td class="px-4 py-2 border">{{ course.PRF_TIPO_PROGRAMA }}</td>
							<td class="px-4 py-2 border">{{ course.modalidad || "null" }}</td>
							<td class="px-4 py-2 border">{{ course.FIC_FCH_INICIALIZACION }}</td>
							<td class="px-4 py-2 border">{{ course.FIC_FCH_FINALIZACION }}</td>
						</tr>
					</tbody>
				</table>
			</div>
		</div>-->
	</div>
</template>

<script setup>
// Simulación de datos desde una API
import { ref, unref, onMounted } from "vue";
import { defineProps } from "vue";
import RutaAtencion from "@/assets/images/ruta.svg";
import Ciudadano from "@/assets/images/Usuario.svg";
import VerLine from "@/assets/images/VerLine.svg";
import Actividad from "@/assets/images/Actividad.png";
import Historial from "@/assets/images/Historial.svg";
import { useEventStore } from "@/stores/storedataOff.js"; // Cambiar según tu estructura de store



const eventStore = useEventStore();
const userInfo = ref({});
const fetchData = ref([]);
const data = ref({});
const host = import.meta.env.VITE_HOST;
const fetchOptions = {
	url: `${host}:8084/api/v1/ciudadano/ValidarVictima`,
	options: {
		method: "POST",
		headers: {
			"Accept": "application/json",
		},
	}
}
// Define los props que espera el componente
defineProps({
	userInfo: {
		type: Object,
		required: true,
	},
});

const getFetchData = async (fetchOptions) => {
	const userInfoData = unref(userInfo); // Extraemos userInfo
	data.value = userInfoData?.data || {};
	console.log('dar se la come dobalda; ', data.value);

	const { url, options } = fetchOptions
	let newUrl = url + `/${userInfo.value.documento}`
	console.log(newUrl);

	try {
		const response = await fetch(newUrl, options);
		const json = await response.json();
		if (!response.ok) throw { error: true, errorStatus: response.status, errorMsg: response.statusText }
		console.log(json)
		fetchData.value = json;

	} catch (error) {
		if (!error.error) error.error = true
		console.log(error)
	} finally {
		newUrl = ''
	}
}

onMounted(() => {
	userInfo.value = eventStore.getUserInfo()
	getFetchData(fetchOptions)
	console.log(fetchData.value);

});


</script>

<style scoped>
table {
	border-collapse: separate;
	border-spacing: 1px 1px;
	/* Espacio entre celdas */
	min-width: 100%;
	margin-top: 1rem;
}

th,
td {
	border: 1px solid #ccc;
	/* Borde individual para cada celda */
	background-color: #fff;
	/* Fondo blanco para celdas */
	text-align: left;
	/* Alinear texto a la izquierda */
}

thead th {
	background-color: #315CA0;
	color: #fff;
	font-weight: bold;
	border-radius: 0.5rem 0.5rem 0rem 0rem;
	overflow: hidden;
	padding: 12px;
}

tbody tr:hover td {
	background-color: #f9f9f9;
	/* Fondo más claro al pasar el cursor */
}

tbody td {
	padding: 10px;
	/* Espaciado interno en las celdas */
}
</style>