<template>
	<div class="min-h-screen md:p-4 flex flex-col  md:-mt-20 -mt-10 ">
		<!-- Sección Superior (Título, Icono, Información del Ciudadano) -->
		<div class=" flex flex-col md:flex-row items-center justify-between md:p-2 rounded-lg  md:max-w-7xl ">
			<!-- Icono y Título -->
			<div class="flex items-center -mb-12 md:-mb-0 ">
				<div class="md:p-6 p-4 bg-customPurple rounded-full ">
					<!-- Icono SVG -->
					<img :src="Reportes" 
					alt=" Icono de Reportes" 
					class="md:w-16 md:h-16 h-8 w-9" />
				</div>
				<div class="md:ml-4 ml-3 text-left md:mt-14">
					<p class="text-black -mb-1 text-2xl md:text-[37px] ">Generar</p>
					<h2 class="text-customPurple text-5xl md:text-[94px] mt-0 font-bold">Reportes</h2>
				</div>
			</div>
		</div>
		<!-- Imagen Izquierda -->
		<div class="flex-1 max-w-md lg:max-w-lg p-3">
			<img :src="PersonaReportes" 
			alt="Persona sonriendo" 
			class="h-[356px] w-[332px] md:hidden" />
		</div>
		<!-- Sección Central (Imagen y Formulario) -->
		<div class=" flex flex-col md:flex-row md:items-start  md:w-full md:max-w-9xl md:space-y-8 xl:space-y-9 xl:space-x-10 relative">
			<!-- Formulario a la Izquierda -->
			<div class="flex-grow  max-w-md lg:max-w-xl  bg-white rounded-lg shadow-md md:w-72 p-3 mb-9 md:-mt-0 w-full relative">
				<center>
					<h3>Seleccione el tipo de reporte</h3>
				</center>
				<!-- Selección de Formato -->
				<div>
					<label class="block text-gray-700 text-sm font-semibold -mb-6 "></label>
					<br>
					<div class="radio-button md:text-base grid grid-flow-col items-center md:mx-9 md:mr-3 text-[13px]">
						<input type="radio" id="orientados" name="role" value="Orientados" class="custom-radio"
							v-model="selectedRole" />
						<label for="orientados">Exportar Orientaciones</label>
						<input type="radio" id="inscritos" name="role" value="Inscritos" class="custom-radio"
							v-model="selectedRole" />
						<label for="inscritos">Exportar Inscritos</label>
						<input type="radio" id="colocados" name="role" value="Colocados" class="custom-radio"
							v-model="selectedRole" />
						<label for="colocados">Exportar Colocados</label>
					</div>

					<br>
					<div class="space-y-2">

						<label class="hidden p-4 rounded-lg shadow-sm border cursor-pointer transition-all duration-300"
							:class="{ 'bg-customPurple text-white font-bold': selectedFormat === 'excel', 'bg-gray-100': selectedFormat !== 'excel' }"
							@click="selectFormat('excel')">
							EXCEL
						</label>

					</div>
				</div>

				<!-- Selección de Departamento -->

				<div class="mb-4">
					<div class="grid grid-wrap">
						<select v-model="selectedDepartamento"
							class="block p-4 rounded-lg focus:outline-none focus:ring-2 font-bold border cursor-pointer text-negro h-12 border-none bg-grisInput"
							id="departamento">
							<option disabled value="">Seleccione Departamento</option>
							<option v-for="departamento in departamentos" :key="departamento.code"
								:value="departamento.code" class="bg-white ">
								{{ departamento.name }}
							</option>
						</select>
					</div>
				</div>

				<!-- Selección de Fechas -->
				<div class="mb-4">
					<label class="block text-negro text-sm font-semibold mb-2">Seleccione el rango de fechas.</label>
					<div class="flex items-center space-x-4">
						<input type="date"
							class="w-1/2 p-2 border rounded text-gray-700 bg-grisInput focus:outline-none focus:ring-2 focus:ring-customPurple"
							placeholder="Desde" v-model="dateRange.from" />
						<input type="date"
							class="w-1/2 p-2 border rounded text-negro bg-grisInput focus:outline-none focus:ring-2 focus:ring-customPurple"
							placeholder="Hasta" v-model="dateRange.to" />
					</div>
				</div>

				<!-- Botón de Búsqueda -->
				<button
					class="w-full bg-customPurple text-lg cursor-pointer border-none text-amarillo font-bold py-2 rounded-lg shadow-md mt-4"
					:disabled="loading" @click="handleDownloadExcel">
					<span v-if="!loading">Generar Reporte</span>
					<span v-else>Generando...</span>
				</button>
			</div>
			<!-- Imagen a la Izquierda -->
			<div class="md:flex-1 md:max-w-md md::max-w-lg md:p-1 hidden md:block">
				<img :src="PersonaReportes" alt="Persona sonriendo" class="md:h-[700px] md:w-[697px] md:-mt-60 " />
			</div>
		</div>
	</div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import Reportes from "@/assets/images/Reportes.svg";
import PersonaReportes from "@/assets/images/PersonaReportes.png";

const selectedRole = ref("");
const selectedFormat = ref("excel");
const selectedDepartamento = ref("");
const dateRange = ref({ from: "", to: "" });
const host = import.meta.env.VITE_HOST;

const departamentos = ref([
	{ name: "Amazonas", code: "91" },
	{ name: "Antioquia", code: "05" },
	{ name: "Arauca", code: "81" },
	{ name: "Atlantico", code: "08" },
	{ name: "Bolivar", code: "13" },
	{ name: "Boyacá", code: "15" },
	{ name: "Caldas", code: "17" },
	{ name: "Caquetá", code: "18" },
	{ name: "Casanare", code: "85" },
	{ name: "Cauca", code: "19" },
	{ name: "Cesar", code: "20" },
	{ name: "Chocó", code: "27" },
	{ name: "Cundinamarca", code: "25" },
	{ name: "Cordoba", code: "23" },
	{ name: "Guainia", code: "94" },
	{ name: "Guaviare", code: "95" },
	{ name: "Huila", code: "41" },
	{ name: "La Guajira", code: "44" },
	{ name: "Magdalena", code: "47" },
	{ name: "Meta", code: "50" },
	{ name: "Nariño", code: "52" },
	{ name: "Norte de Santander", code: "54" },
	{ name: "Putumayo", code: "86" },
	{ name: "Quindio", code: "63" },
	{ name: "Risaralda", code: "66" },
	{ name: "San Andres, Providencia y Santa Catalina", code: "88" },
	{ name: "Santander", code: "68" },
	{ name: "Sucre", code: "70" },
	{ name: "Tolima", code: "73" },
	{ name: "Valle del Cauca", code: "76" },
	{ name: "Vaupés", code: "97" },
	{ name: "Vichada", code: "99" },
]);
const loading = ref(false);

function selectFormat(format) {
	selectedFormat.value = format;
}

function validateInputs() {
	if (!selectedRole.value || !selectedFormat.value || !selectedDepartamento.value) {
		alert("Por favor, complete todos los campos.");
		return false;
	}
	if (!dateRange.value.from || !dateRange.value.to) {
		alert("Por favor, seleccione un rango de fechas.");
		return false;
	}
	return true;

}

function getEndpoint() {
	const endpoints = {
		Orientados: `${host}:8084/api/v1/orientaciones/exportarOrientaciones`,
		Inscritos: `${host}8084/api/v1/archivo/exportarInscritos`,
		Colocados: `${host}8084/api/v1/archivo/exportarColocadps`,
	};
	console.log('rol seleccionado', selectedRole.value);

	return endpoints[selectedRole.value];
}

async function handleDownloadExcel() {
	if (!validateInputs()) return;

	loading.value = true;
	const endpoint = getEndpoint();
	const payload = {
		departamento: selectedDepartamento.value,
		dateRange: dateRange.value,
	};

	try {
		// Hacer la solicitud para generar el archivo
		const response = await axios.get(endpoint, payload);

		// Verificar si la respuesta incluye la ruta local del archivo
		const { file } = response.data;
		console.log('nombre archivo: ', file);

		if (!file) {
			throw new Error("El servidor no devolvió una ruta para descargar el archivo.");
		}

		// Leer el archivo local usando una solicitud fetch
		const responseFile = await fetch(file);
		if (!responseFile.ok) {
			throw new Error("No se pudo acceder al archivo local.");
		}

		const blob = await responseFile.blob();

		// Crear un enlace para descargar el archivo
		const downloadLink = document.createElement("a");
		const url = URL.createObjectURL(blob);

		downloadLink.href = url;
		downloadLink.setAttribute("download", file.split("/").pop()); // Extraer el nombre del archivo
		document.body.appendChild(downloadLink);
		console.log('Datos  de Descarga: ', downloadLink);

		downloadLink.click();
		downloadLink.remove();
		URL.revokeObjectURL(url);
	} catch (error) {
		console.error("Error al generar el reporte:", error);
	} finally {
		loading.value = false;
	}
}

</script>

<style scoped>
/* Estilo para radio buttons personalizados */
.radio-button input[type="radio"].custom-radio {
	appearance: none;
	-webkit-appearance: none;
	-moz-appearance: none;
	width: 16px;
	height: 16px;
	border: 2px solid #315CA0;
	/* Color morado del borde */
	border-radius: 50%;
	margin-right: 0.5rem;
	outline: none;
	cursor: pointer;
}

.radio-button input[type="radio"].custom-radio:checked {
	background-color: #315CA0;
	/* Fondo morado al seleccionar */
	border-color: #315CA0;
	/* Asegura que el borde sea morado al seleccionarse */
}

.radio-button label {
	cursor: pointer;
	font-weight: 500;
	margin-right: 1rem;
	color: #333;
	/* Color del texto */
}

.radio-button input[type="radio"].custom-radio:checked+label {
	color: #080808;
	/* Color del texto cuando se selecciona */
	font-weight: 700;
}

option {
	font-size: 14px;
	/* Cambia el tamaño de la fuente de las opciones */
	padding: 4px;
	/* Reduce el espacio interno */
	line-height: 1.2;
	/* Ajusta el espaciado entre líneas */
}
</style>