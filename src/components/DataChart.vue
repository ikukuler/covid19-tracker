<template>
	<div v-if="!loading" class="mt-10 w-full shadow-md p-4 mb-5 border">
		<selector :items="selectorOptions" name="Type" @select="displaySelected" />
		<chart :options="chartOptions" />
	</div>
	<loader v-else />
</template>

<script>
import Selector from "@/components/Selector";
import Loader from "@/components/Loader";
import moment from "moment";
import { Chart } from "highcharts-vue";

const COLORS = {
	confirmed: "#84bafd",
	deaths: "#333",
	cases: "#84bafd",
	recovered: "#047857",
};

export default {
	name: "DataChart",
	props: ["slug", "name"],
	components: {
		Selector,
		Chart,
		Loader,
		Loader,
	},
	data() {
		return {
			covidData: {},
			selectorOptions: [
				{
					ID: "cases",
					value: "cases",
					displayName: "Cases",
				},
				{
					ID: "deaths",
					value: "deaths",
					displayName: "Deaths",
				},
				{
					ID: "recovered",
					value: "recovered",
					displayName: "Recovered",
				},
			],
			selectedState: {
				value: "cases",
				displayName: "Cases",
				color: COLORS.confirmed,
			},
			loading: true,
		};
	},

	methods: {
		async fetchChartData() {
			const url = `https://disease.sh/v3/covid-19/historical/${this.slug}?lastdays=all`;
			const res = await fetch(url);
			const { timeline } = await res.json();
			this.covidData = timeline;
			this.loading = false;
		},

		buildSeries(state) {
			const selectedData = this.covidData[state];
			let data = [];
			for (let key in selectedData) {
				data.push([moment(key).valueOf(), selectedData[key]]);
			}
			return data;
		},

		displaySelected(state) {
			if (!state) return;
			this.selectedState = {
				value: state.value,
				displayName: state.displayName,
				color: COLORS[state.value],
			};
		},
	},

	computed: {
		chartOptions() {
			return {
				title: {
					text: this.name,
				},
				chart: {
					type: "line",
				},
				xAxis: {
					type: "datetime",
				},
				yAxis: {
					title: {
						text: "Cases",
					},
				},
				plotOptions: {
					series: {
						color: this.selectedState.color,
					},
				},
				series: [
					{
						name: this.selectedState.displayName,
						data: this.buildSeries(this.selectedState.value),
					},
				],
			};
		},
	},

	watch: {
		slug: "fetchChartData",
	},

	async mounted() {
		this.fetchChartData();
	},
};
</script>
