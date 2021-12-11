<template>
	<div class="grid md:grid-cols-2 gap-4">
		<div class="bg-blue-100 shadow-md p-10 text-center rounded">
			<h3 class="text-3xl text-blue-900 font-bold mb-4">Cases</h3>
			<div class="text-2xl mb-4">
				<span class="font-bold">New: </span>
				<span>{{ formatNumbers(stats.NewConfirmed) }}</span>
			</div>
			<div class="text-2xl mb-4">
				<span class="font-bold">Total: </span>
				<span>{{ formatNumbers(stats.TotalConfirmed) }}</span>
			</div>
		</div>
		<div class="bg-blue-200 shadow-md p-10 text-center rounded">
			<h3 class="text-3xl text-blue-900 font-bold mb-4">Deaths</h3>
			<div class="text-2xl mb-4">
				<span class="font-bold">New: </span>
				<span>{{ formatNumbers(stats.NewDeaths) }}</span>
			</div>
			<div class="text-2xl mb-4">
				<span class="font-bold">Total: </span>
				<span>{{ formatNumbers(stats.TotalDeaths) }}</span>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "DataBoxes",
	props: ["stats"],
	methods: {
		formatNumbers(num) {
			return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
		},
	},
};
</script>
