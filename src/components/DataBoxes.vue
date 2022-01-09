<template>
	<div class="grid md:grid-cols-3 gap-4">
		<div class="bg-blue-100 shadow-md p-10 text-center rounded">
			<h3 class="text-3xl text-blue-900 font-bold mb-4">Cases</h3>
			<div class="text-2xl mb-4">
				<span class="font-bold">New: </span>
				<span>{{ formatNumbers(stats.todayCases) }}</span>
			</div>
			<div class="text-2xl mb-4">
				<span class="font-bold">Total: </span>
				<span>{{ formatNumbers(stats.cases) }}</span>
			</div>
		</div>
		<div class="bg-blue-200 shadow-md p-10 text-center rounded">
			<h3 class="text-3xl text-blue-900 font-bold mb-4">Deaths</h3>
			<div class="text-2xl mb-4">
				<span class="font-bold">New: </span>
				<span>{{ formatNumbers(stats.todayDeaths) }}</span>
			</div>
			<div class="text-2xl mb-4">
				<span class="font-bold">Total: </span>
				<span>{{ formatNumbers(stats.deaths) }}</span>
			</div>
		</div>
		<div class="bg-blue-300 shadow-md p-10 text-center rounded">
			<h3 class="text-3xl text-blue-900 font-bold mb-4">Tests</h3>
			<div class="text-2xl mb-4">
				<span class="font-bold">Tests per 1M people: </span>
				<span>{{ formatNumbers(stats.testsPerOneMillion) }}</span>
			</div>
			<div class="text-2xl mb-4">
				<span class="font-bold">Total: </span>
				<span>{{ formatNumbers(stats.tests) }}</span>
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
