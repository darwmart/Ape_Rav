<template>
	<PerfilCiudadano :user-info="userInfo" />
</template>

<script setup>
import { computed } from "vue";
import { useRoute } from "vue-router";
import PerfilCiudadano from "@/components/PerfilCiudadano.vue";

// Accede a la ruta actual
const route = useRoute();

// Obtén los parámetros desde la ruta
const userInfo = computed(() => route.params.userInfo);
</script>
