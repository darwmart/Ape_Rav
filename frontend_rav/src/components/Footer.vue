<template>
	<footer class="w-full h-12 bg-azulHeaderFooter flex items-center justify-between px-4 mt-4">
		<!-- Ícono institucional a la izquierda con fondo azul -->

		<div
			class="flex items-center  w-full sm:w-auto sm:justify-start bg-azulHeaderFooter">
			<img
				:src="IconoGov"
				alt="Icono Institucional"
				class="w-[140px] h-6 ml-2 sm-ml-0 my-1" />
		</div>
	</footer>
</template>

<script setup>
// Importamos el componente del ícono
import IconoGov from "@/assets/images/Gov.svg";
</script>
