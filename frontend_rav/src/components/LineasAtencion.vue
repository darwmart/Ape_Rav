<template>
	<div class="min-h-screen md:p-4 flex flex-col  md:-mt-20 -mt-10 ">
		<!-- Sección Superior (Título, Icono, Información del Ciudadano) -->
		<div class="flex flex-col md:flex-row items-center justify-between md:p-2 rounded-lg md:max-w-7xl">
			<!-- Icono y Título -->
			<div class="flex items-center mb-3 md:-mb-0 ">
				<div class="md:p-6 bg-azulBarraApe p-4 rounded-full">
					<!-- Icono SVG -->
					<img :src="lineaAten" 
					alt=" Icono de Lineas de atencion" 
					class="md:w-16 md:h-16 h-8 w-9" />
				</div>
				<div class="md:ml-4 ml-3 text-left md:mt-14">
					<p class="text-black -mb-1 text-2xl md:text-[37px]">Lineas de</p>
					<h2 class="text-customPurple text-5xl md:text-[94px] mt-0 font-bold">Atención</h2>
				</div>
			</div>
		</div>

		<!-- Sección de Selección de Entidades -->
		<div class="md:flex-col md:gap-7 md:p-4  grid grid-col gap-2">
			<!-- Subtítulo de Selección -->
			<div class="flex items-center space-x-1 flex-auto -mb-5">
				<img :src="select1" alt="Icono de selección 1" class="w-5 h-5 " />
				<p class="font-sans text-sm md:text-xl">Seleccione la</p>
				<h3 class="font-sans text-sm md:text-xl">Entidad</h3>
			</div>

			<!-- Contenedor de Imágenes de Entidades -->
			<div class="md:flex md:justify-between grid grid-cols-2  items-center gap-x-3 gap-y-8 p-2 ">
				<!-- SENA -->
				<div class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer md:p-2 shadow-md h-24 w-42 justify-center"
					:class="{ 'ring ring-customPurple': selectedEntity === 'sena' }" @click="() =>
							handleEntityClick({
								image: LogoSenaverde,
								sede: sedes[0],
								newEntity: 'sena',
							})
						">
					<img :src="LogoSenaverde" alt="Logo Verde SENA"
						class="md:w-20 md:h-40 object-center md:-mt-1 w-40 h-16 mt-4" />
				</div>

				<!-- APE -->
				<div class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer  shadow-md h-24 w-40 "
					:class="{ 'ring ring-customPurple': selectedEntity === 'ape' }" @click="() =>
							handleEntityClick({
								image: LogoApe,
								sede: sedes[1],
								newEntity: 'ape',
							})
						">
					<img :src="LogoApe" alt="Logo APE" class="object-scale-down md:h-48 md:w-64 w-44 h-12 mt-7 " />
				</div>

				<!-- Certificaciones -->
				<div class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer md:p-2 shadow-md h-24 w-42 px-2"
					:class="{
						'ring ring-customPurple': selectedEntity === 'certificaciones',
					}" @click="() =>
							handleEntityClick({
								image: LogCertLab,
								sede: sedes[2],
								newEntity: 'certificaciones',
							})
						">
					<img :src="LogCertLab" alt="Certificados Laborales"
						class="object-scale-down md:h-48 md:w-64  w-36 h-5 mt-10 mb-4" />
				</div>

				<!-- Fondo Emprender -->
				<div class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer p-2 shadow-md h-24 w-42"
					:class="{
						'ring ring-customPurple': selectedEntity === 'fondoEmprender',
					}" @click="() =>
							handleEntityClick({
								image: FondoEmprender,
								sede: sedes[3],
								newEntity: 'fondoEmprender',
							})
						">
					<img :src="FondoEmprender" alt="Fondo Emprender"
						class="object-scale-down md:h-48 md:w-64 object-center w-36 h-7 mt-8 mb-2" />
				</div>
			</div>

			<!-- Sección de Selección de Regional -->
			<div class="flex items-center space-x-1 flex-auto -mb-3">
				<img :src="select2" alt="Icono de Selección 2" class="w-5 h-5 "  />
				<p class="font-sans">Seleccione la</p>
				<h3 class="font-sans font-bold">Regional</h3>
			</div>

			<!-- Dropdown de Departamentos -->
			<div class="flex items-center space-x-2 h-1hadow-md">
				<Select v-model="selectedCountry" :options="departamentos" placeholder="Seleccione departamento"
					class="w-full rounded-lg shadow-sm text-customPurple !border !border-customPurple"
					@change="updateSelectedInfo">
					<template #value="slotProps">
						<div v-if="slotProps.value" class="flex gap-2 items-center font-semibold text-customPurple">
							<div>{{ slotProps.value.name }}</div>
						</div>
						<span v-else class="text-customPurple">{{
							slotProps.placeholder
						}}</span>
					</template>
					<template #option="slotProps">
						<div class="flex items-center font-medium text-customPurple">
							<p>{{ slotProps.option.name }}</p>
						</div>
					</template>
				</Select>
			</div>

			<!-- Botón de Búsqueda -->
			<div class="flex items-center justify-center">
				<Button
					class="w-72 !bg-customPurple text-lg cursor-pointer border-none !text-amarillo font-bold py-2 rounded-lg shadow-md mt-4"
					type="button" label="Buscar" icon="pi pi-search" :loading="loading" @click="validateAndSearch" />
			</div>

			<!-- Componente de PopUp -->
			<PopUp :data="fetchData" :isOpen="isModalOpen" :image="selectedImage" :sede="selectedSede"
				:departamento="selectedCountry?.name" @close="isModalOpen = false" />
		</div>

		<Dialog v-model:visible="noResultsModal" modal header="Búsqueda Fallida" :style="{ width: '30rem' }"
			:closable="true" :draggable="true">
			<p class="m-0 text-customPurple">
				Por favor seleccione una entidad y un departamento para continuar con la
				búsqueda.
			</p>
			<template #footer>
				<Button label="Entendido" @click="noResultsModal = false" class="p-button-text" />
			</template>
		</Dialog>
	</div>
</template>

<script setup>
import { ref } from "vue";
import select1 from "@/assets/imgLineasAtencion/select1.svg";
import select2 from "@/assets/imgLineasAtencion/select2.svg";
import lineaAten from "@/assets/imgLineasAtencion/lineaAten.svg";
import LogoSenaverde from "@/assets/images/logosenaverde.svg";
import LogCertLab from "@/assets/imgLineasAtencion/LogCertLab.svg";
import LogoApe from "@/assets/images/LogoApe.png";
import FondoEmprender from "@/assets/images/FondoEmprender.png";
import Select from "primevue/select";
import PopUp from "./PopUp.vue";
import Button from "primevue/button";

const selectedCountry = ref("");
const selectedInfo = ref(null);
const selectedImage = ref(null);
const selectedSede = ref("");
const isModalOpen = ref(false);
const fetchData = ref([]);
const loading = ref(false);
const selectedEntity = ref(null);
const noResultsModal = ref(false);
const host = import.meta.env.VITE_HOST;

// const selectedDepart = ref (null);
let entity;

const sedes = [
	"SERVICIO NACIONAL DE APRENDIZAJE SENA",
	"AGENCIA PUBLICA DE EMPLEO",
	"CERTIFICACION COMPETENCIAS LABORALES",
	"FONDO EMPRENDER",
];

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

const validateAndSearch = async () => {
	if (!selectedEntity.value || !selectedCountry.value) {
		noResultsModal.value = true; // Mostrar modal si faltan datos
		return;
	}


	// Construir la URL dinámica
	const fetchOptions = {
		url: `${host}:8082/api/v1/victimas/lines/${selectedEntity.value}/${selectedCountry.value.name}`,
		options: {
			method: "GET",
			headers: {
				Accept: "application/json",
			},
		},
	};

	console.log("Realizando búsqueda...");

	loading.value = true; // Mostrar indicador de carga
	try {
		const response = await fetch(fetchOptions.url, fetchOptions.options);
		if (!response.ok) throw new Error("Error en la solicitud");

		const data = await response.json();
		console.log("Datos obtenidos:", data);

		// Aquí puedes actualizar el estado con los datos recibidos
		fetchData.value = data;
		isModalOpen.value = true; // Mostrar resultados en un modal
	} catch (error) {
		console.error("Error al realizar la búsqueda:", error);
	} finally {
		loading.value = false; // Ocultar indicador de carga
	}
};

const updateSelectedInfo = () => {
	const selected = departamentos.value.find(
		(dep) => dep.code === selectedCountry.value?.code
	);
	selectedInfo.value = selected ? selected : null;
	console.log(selectedInfo.value);
};

const setEntity = (newEntity) => {
	entity = newEntity;
	console.log(entity);
};

const selectImage = (image) => {
	selectedImage.value = image;
};

const selectSede = (sede) => {
	selectedSede.value = sede;
};

const searchDepartamentt = async (fetchOptions) => {
	const { url, options } = fetchOptions;
	let newUrl = url + `/${entity}/${selectedInfo.value.name}`;
	console.log(newUrl);

	loading.value = true;

	try {
		// Agregamos un delay artificial (opcional)
		await new Promise((resolve) => setTimeout(resolve, 800)); // 800ms de delay

		const response = await fetch(newUrl, options);
		const json = await response.json();
		if (!response.ok)
			throw {
				error: true,
				errorStatus: response.status,
				errorMsg: response.statusText,
			};
		console.log(json);
		fetchData.value = json;
		isModalOpen.value = true;
	} catch (error) {
		if (!error.error) error.error = true;
		console.log(error);
	} finally {
		// Desactivamos el loading al finalizar
		loading.value = false;
		newUrl = "";
	}
};

const handleEntityClick = (options) => {
	const { image, sede, newEntity } = options;
	setEntity(newEntity);
	selectImage(image);
	selectSede(sede);
	selectedEntity.value = newEntity; // Marcar como seleccionada
};
</script>

<style scoped>
.ring {
	box-shadow: 0 0 0 2px #315CA0;
	/* Tamaño y color del ring */
	border-radius: 0.5rem;
	/* Ajustar el radio según el diseño */
	transition: box-shadow 0.2s ease-in-out;
	/* Suavidad en la transición */
}

.ring-customPurple {
	box-shadow: 0 0 0 2px #315CA0;
	/* Color personalizado */
}
</style>