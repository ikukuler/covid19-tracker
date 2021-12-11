<template>
	<main class="flex flex-col align-center justify-center text-center">
		<p class="text-gray mt-10 mb-6 text-2xl">Fetching...</p>
		<img :src="loadingImage" class="w-24 m-auto" alt="loading" />
	</main>
</template>

<script>
export default {
	name: "Loader",
	data() {
		return {
			loadingImage: require("../assets/loading.gif"),
		};
	},
};
</script>
