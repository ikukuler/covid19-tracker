<template>
	<div class="text-center">
		<h2 class="font-bold text-3xl">{{ text }}</h2>
		<div class="text-2xl mt-3">{{ formattedDate }}</div>
		<div class="text-1xl mt-4 mb-10">
			Population: <span class="font-bold">{{ formattedPopulation }}</span>
		</div>
	</div>
</template>

<script>
import moment from "moment";
export default {
	name: "DataTitle",
	props: ["text", "date", "population"],
	computed: {
		formattedDate() {
			return moment(this.date).format("MMMM Do YYYY, hh:mm:ss");
		},
		formattedPopulation() {
			return this.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
		},
	},
};
</script>
