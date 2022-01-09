<template>
	<main v-if="!loading">
		<selector
			@select="selectCountry"
			:items="formattedCountries"
			name="Country"
		/>

		<data-title :text="title" :date="date" :population="population" />
		<data-boxes :stats="stats" />
		<h2 v-if="slug" class="text-2xl font-bold text-center mt-10">
			Total cases in {{ stats.country }} from the first recorded case
		</h2>
		<data-chart v-if="slug" :slug="slug" :name="stats.country" />

		<button
			v-if="stats.country"
			class="bg-green-700 text-white rounded p-3 mt-10 mb-5 focus:outline-none hover:bg-green-600"
			@click="clearCountry"
		>
			Clear Country
		</button>
	</main>

	<loader v-else />
</template>

<script>
import DataTitle from "@/components/DataTitle";
import DataBoxes from "@/components/DataBoxes";
import Selector from "@/components/Selector";
import DataChart from "@/components/DataChart";
import Loader from "@/components/Loader";
export default {
	name: "Home",
	components: { DataTitle, DataBoxes, Selector, DataChart, Loader },
	data() {
		return {
			loading: true,
			title: "Global",
			date: "",
			population: "",
			stats: {},
			countries: [],
			slug: null,
			loadingImage: require("../assets/loading.gif"),
		};
	},
	methods: {
		async fetchCovidData() {
			const res = await fetch("https://disease.sh/v3/covid-19/all");
			const data = await res.json();
			return data;
		},

		async fetchCountries() {
			const res = await fetch("https://disease.sh/v3/covid-19/countries");
			const data = await res.json();
			return data;
		},

		selectCountry(country) {
			if (!country) {
				this.clearCountry();
				return;
			}
			this.stats = country;
			this.title = country.country;
			this.population = country.population;
			this.slug = country.countryInfo.iso3;
		},

		async clearCountry() {
			this.loading = true;
			const data = await this.fetchCovidData();
			this.stats = data;
			this.title = "Global";
			this.population = data.population;
			this.loading = false;
			this.slug = null;
		},
	},

	computed: {
		formattedCountries() {
			return this.countries.map((country) => {
				return {
					...country,
					ID: country.countryInfo._id,
					displayName: country.country,
					value: country.countryInfo._id,
				};
			});
		},
	},
	async mounted() {
		const data = await this.fetchCovidData();
		const countryData = await this.fetchCountries();
		this.date = data.updated;
		this.stats = data;
		this.population = data.population;
		this.countries = countryData;
		this.loading = false;
	},
};
</script>
