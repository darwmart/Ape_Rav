<template>
    <div class="grid grid-cols-2">
        <div class="flex items-center justify-center  ">
            <img src="@/assets/images/CiudadanoRutaAtencion.svg" alt="Persona sonriendo"
                class="rounded-lg object-cover h-[782px] md:mt-12" />
        </div>
    <div class="block lg:flex items-center justify-center p-6 h-full">
        <div class="lg:pl- lg:pr-10">
            <div class="-mb-4 w-full h-">
                <p class="lg:text-4xl font-medium -mb-10">
                    Aquí podrás visualizar información acerca de la
                </p>
                <h1 class="tracking-wide text-7xl font-bold text-azulBarraApe">
                    Ruta de Atención al Ciudadano.
                </h1>
            </div>
            <div class="w-full mb-30 bg-white p-10 rounded-2xl shadow-lg">
                <div class="flex flex-col items-center">
                    <!-- Nuevo Select para Tipo de Identificación -->
                    <select v-model="selectedTipoIdentificacion"
                        class="w-full p-4 mb-3 border text-black border-none rounded-lg bg-grisFondo">
                        <option disabled value="">Seleccione tipo de identificación</option>
                        <option value="CC">Cédula de Ciudadanía</option>
                        <option value="TI">Tarjeta de Identidad</option>
                    </select>

                    <!-- Input para el Número de Documento -->
                    <input v-model="searchCedula" @keydown.enter="searchUser" type="text"
                        placeholder="Ingrese número de documento"
                        class="w-full p-4 border text-black border-none rounded-lg bg-grisFondo" />
                    <Button type="button" label="Buscar" icon="pi pi-search" :loading="loading"
                        class="mt-3 w-full hover:bg-azulBarraApe !border-none !bg-azulBarraApe !text-yellow-400"
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

async function searchByCedula(tipoId, cedula) {
    const body = {
        tipo_identificacion: tipoId, // Reemplaza con el valor correspondiente
        identificacion: cedula, // Reemplaza con el valor correspondiente
    };
    try {
        const response = await axios.post(
            'http://localhost:5010/api/v1/ciudadano/ValidarVictima',
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
