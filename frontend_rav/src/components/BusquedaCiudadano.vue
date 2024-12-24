<template>
    <div class="md:grid md:grid-cols-2 grid grid-cols-1 relative">
        <!-- Contenedor del Título -->
        <div class="absolute -mt-8 left-0 w-full p-1   md:hidden">
            <p class="text-xl font-medium text-center">
                Aquí podrás visualizar información acerca de la
            </p>
            <h1 class="text-3xl font-bold text-azulBarraApe text-center">
                Ruta de Atención al Ciudadano
            </h1>
        </div>

        <!-- Contenedor de la Imagen -->
        <div class="md:flex md:items-center md:justify-center relative md:mt-0 mt-36 -mb-14">
            <img src="@/assets/images/CiudadanoRutaAtencion.svg" alt="Persona sonriendo"
                class="md:rounded-lg md:object-cover md:h-[782px] md:mt-12 h-[472px] " />
            
            <!-- Formulario encima de la imagen en pantallas pequeñas -->
            <div class="absolute top-0 left-0 w-full h-full flex items-center justify-center md:hidden">
                <div class="bg-white bg-opacity-80 p-6 rounded-lg shadow-lg  mt-44">
                    <!-- Select para Tipo de Identificación -->
                    <select v-model="selectedTipoIdentificacion"
                        class="w-full h-4 p-4 mb-2 border text-black rounded-lg bg-grisFondo">
                        <option disabled value="">Seleccione tipo de identificación</option>
                        <option value="CC">Cédula de Ciudadanía</option>
                        <option value="TI">Tarjeta de Identidad</option>
                    </select>

                    <!-- Input para el Número de Documento -->
                    <input v-model="searchCedula" @keydown.enter="searchUser" type="text"
                        placeholder="Ingrese número de documento"
                        class="w-full h-4 p-4 border text-black rounded-lg bg-grisFondo mb-3" />
                    
                    <Button type="button" label="Buscar" icon="pi pi-search" :loading="loading"
                        class="w-full hover:bg-azulBarraApe !border-none !bg-azulBarraApe !text-yellow-400"
                        @click="searchUser" />
                </div>
            </div>
        </div>

        <!-- Contenedor del Texto y Formulario en Pantallas Grandes -->
        <div class="md:flex md:items-center md:justify-center md:p-6 md:h-full hidden ">
            <div class="lg:pl- lg:pr-10">
                <div class="-mb-4 w-full h-">
                    <p class="md:text-4xl md:font-medium md:-mb-10 text-xl">
                        Aquí podrás visualizar información acerca de la
                    </p>
                    <h1 class="md:tracking-wide md:text-7xl font-bold text-azulBarraApe">
                        Ruta de Atención al Ciudadano.
                    </h1>
                </div>
                <div class="w-full mb-30 bg-white p-10 rounded-2xl shadow-lg">
                    <div class="flex flex-col items-center">
                        <select v-model="selectedTipoIdentificacion"
                            class="w-full p-4 mb-3 border text-black rounded-lg bg-grisFondo">
                            <option disabled value="">Seleccione tipo de identificación</option>
                            <option value="CC">Cédula de Ciudadanía</option>
                            <option value="TI">Tarjeta de Identidad</option>
                        </select>
                        <input v-model="searchCedula" @keydown.enter="searchUser" type="text"
                            placeholder="Ingrese número de documento"
                            class="w-full p-4 border text-black rounded-lg bg-grisFondo mb-3" />
                        <Button type="button" label="Buscar" icon="pi pi-search" :loading="loading"
                            class="w-full hover:bg-azulBarraApe !border-none !bg-azulBarraApe !text-yellow-400"
                            @click="searchUser" />
                    </div>
                </div>
            </div>
        <Dialog v-model:visible="noResultsModal" modal header="Búsqueda Fallida" :style="{ width: '30rem' }">
            <p class="m-0 text-azulBarraApe">
                {{ modalMessage }}
                <strong>{{ searchCedula }}</strong>
            </p>
            <template #footer>
                <Button label="Cerrar" @click="noResultsModal = false" class="p-button-text" />
            </template>
        </Dialog>
    </div>
    
    </div>
</template>

<script setup>
import CiudadanoRutaAtencion from '@/assets/images/CiudadanoRutaAtencion.svg';
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useEventStore } from "@/stores/storedataOff.js";
import Dialog from "primevue/dialog";
import Button from "primevue/button";
import axios from "axios";

const searchCedula = ref("");
const selectedTipoIdentificacion = ref("");
const noResultsModal = ref(false);
const modalMessage = ref("");
const loading = ref(false);
const router = useRouter();
const eventStore = useEventStore();
const host = import.meta.env.VITE_HOST;

async function searchByCedula(tipoId, cedula) {
    const body = {
        tipo_identificacion: tipoId, // Reemplaza con el valor correspondiente
        identificacion: cedula, // Reemplaza con el valor correspondiente
    };
    try {
        const response = await axios.post(
            `${host}:5010/api/v1/ciudadano/ValidarVictima`,
            body,
            {
                headers: {
                    'Content-Type': 'application/json', // Asegura que el servidor entienda el formato
                },
            }
        );
        return response.data; // Aquí accedemos directamente a los datos JSON
    } catch (error) {
        console.error("Error al buscar eventos por cédula:", error);
        return [];
    }
}

const searchUser = async () => {
    if (!selectedTipoIdentificacion) {
        modalMessage.value = 'Por favor seleccione un tipo de identificación.';
        noResultsModal.value = true;
        return;
    }
    if (!searchCedula.value.trim()) {
        noResultsModal.value = true;
        modalMessage.value = "Por favor, ingrese un número de documento.";
        return;
    }
    loading.value = true;
    console.log("lo que sea: ",selectedTipoIdentificacion.value);
    
    const results = await searchByCedula(selectedTipoIdentificacion.value, searchCedula.value);

    if (Object.keys(results).length > 0) {
        eventStore.setUserInfo(results);
        router.push({
            name: "PerfilCiudadanoPage",
            params: { userInfo: results },
        });
    } else {
        noResultsModal.value = true;
        modalMessage.value = `No se encontraron resultados con la cédula: ${searchCedula.value}`;
    }

    loading.value = false;
};
</script>
