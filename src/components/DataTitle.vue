<template>
	<div class="text-center">
		<h2 class="font-bold text-3xl">{{ text }}</h2>
		<div class="mt-4 mb-10 text-2xl">{{ formattedDate }}</div>
	</div>
</template>

<script>
import moment from "moment";
export default {
	name: "DataTitle",
	props: ["text", "date"],
	computed: {
		formattedDate() {
			return moment(this.date).format("MMMM Do YYYY, hh:mm:ss");
		},
	},
};
</script>
