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

const formatData = (data) => {
	return data.map((item) => {
		return {
			date: moment(item.Date).valueOf(),
			deaths: item.Deaths,
			confirmed: item.Confirmed,
			recovered: item.Recovered,
			active: item.Active,
		};
	});
};

const COLORS = {
	confirmed: "#84bafd",
	deaths: "#333",
	active: "#1e40af",
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
			covidData: [],
			selectorOptions: [
				{
					ID: "confirmed",
					value: "confirmed",
					displayName: "Confirmed",
				},
				{
					ID: "deaths",
					value: "deaths",
					displayName: "Deaths",
				},
				{
					ID: "active",
					value: "active",
					displayName: "Active",
				},
				{
					ID: "recovered",
					value: "recovered",
					displayName: "Recovered",
				},
			],
			selectedState: {
				value: "confirmed",
				displayName: "Confirmed",
				color: COLORS.confirmed,
			},
			loading: true,
		};
	},

	methods: {
		async fetchChartData() {
			const url = `https://api.covid19api.com/dayone/country/${this.slug}`;
			const res = await fetch(url);
			const data = await res.json();
			this.covidData = data;
			this.loading = false;
			return data;
		},

		buildSeries(state) {
			const formatted = formatData(this.covidData);
			return formatted.map((item) => {
				return [item.date, item[state]];
			});
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

	mounted() {
		this.fetchChartData();
	},
};
</script>
