<template>
	<Header />
	<div class="min-h-screen bg-azulBarraApe text-white flex flex-col"		
			:style="{
			backgroundImage: backgroundUrl,
			backgroundSize: 'cover',
			backgroundPosition: 'center',
		}">
		<!-- Columna izquierda con el formulario de login -->
		<div
			class="md:w-1/2 w-full flex flex-col justify-center items-center p-6 bg-black/70 md:bg-transparent my-36">
			<!-- Logo -->
			<img :src="Logo" alt="Logo APE"  class="w-64 max-w-sm h-auto"  />

			<!-- Título de inicio de sesión -->
			<h2 class="text-2xl sm:text-3xl font-semibold text-center">
				Iniciar Sesión
			</h2>

			<!-- Formulario de login -->
			<form
				@submit.prevent="submit"
				class="space-y-4 w-full max-w-sm mx-auto mt-6">
				<div>
					<input
						type="email"
						id="correo"
						placeholder="Correo"
						v-model="form.email"
						class="block w-full bg-white text-gray-800 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-customPurple"
						required />
				</div>

				<div>
					<input
						type="password"
						id="password"
						placeholder="Contraseña"
						v-model="form.password"
						class="block w-full bg-white text-gray-800 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-customPurple"
						required />
				</div>

				<!-- Botón de iniciar sesión -->
				<button
					type="submit"
					class="w-full py-3 text-lg bg-amarillo border-none text-customPurple font-bold cursor-pointer rounded-lg"
					:disabled="isLoading">
					<span v-if="!isLoading">Iniciar</span>
					<span v-else>Cargando...</span>
				</button>
			</form>

			<!-- Logos institucionales -->
			<div class="flex justify-center space-x-8 mt-6">
				<img :src="logoSena" alt="Logo SENA" class="w-24 md:w-36 h-auto" />
			</div>
		</div>

		<!-- Right column with background image -->
		<div class="hidden md:block md:w-1/2"></div>
	</div>
	<Footer />
</template>

<script setup>
import { reactive, ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import { useAuthStore } from "../stores/auth";
import axios from "axios";
import { useToast } from "vue-toastification";
import Logo from "@/assets/images/LogoApe.svg";
import logoSena from "@/assets/images/logosInstitucionales.svg";
import loginFinal from '@/assets/images/bgApe.png';
import smallScreenLogin from '@/assets/images/bgApe.png';

import Header from "../components/Header.vue";
import Footer from "../components/Footer.vue";

const form = reactive({
	email: "",
	password: "",
});

const host = import.meta.env.VITE_HOST;

const authStore = useAuthStore();
const errorMessage = ref("");
const isLoading = ref(false);
const backgroundUrl = ref(""); // Variable para almacenar la URL de fondo
const router = useRouter();
const toast = useToast();

onMounted(() => {
	// Detecta el ancho de la pantalla después de que el componente ha sido montado
	const screenWidth = window.innerWidth;
	backgroundUrl.value =
		screenWidth <= 640 ? `url(${smallScreenLogin})` : `url(${loginFinal})`;

	// Opcional: Actualizar el fondo al redimensionar la ventana
	window.addEventListener("resize", () => {
		const newScreenWidth = window.innerWidth;
		backgroundUrl.value =
			newScreenWidth <= 640 ? `url(${smallScreenLogin})` : `url(${loginFinal})`;
	});
});

async function submit() {
	isLoading.value = true;
	try {
		const response = await axios.post(`${host}:8080/login`, form);
		console.log(response.data);

		authStore.setAuthenticatedUser(response.data);
		toast.success("Inicio de sesión exitoso.");
		router.push("/");
	} catch (error) {
		errorMessage.value =
			error.response?.data?.error || "Error en el inicio de sesión.";
	} finally {
		isLoading.value = false;
	}
}
</script>

<style scoped>
button {
	transition: background-color 0.3s ease;
}

input {
	border: none;
	/* Quitar bordes visibles */
}

input:focus {
	outline: none;
}
</style>
