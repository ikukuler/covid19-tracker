<template>
	<main v-if="!loading">
		<selector
			@select="selectCountry"
			:items="formattedCountries"
			name="Country"
		/>

		<data-title :text="title" :date="date" />
		<data-boxes :stats="stats" />
		<h2 v-if="slug" class="text-2xl font-bold text-center mt-10">
			Total cases in {{ stats.Country }} from the first recorded case
		</h2>
		<data-chart v-if="slug" :slug="slug" :name="stats.Country" />

		<button
			v-if="stats.Country"
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
			stats: {},
			countries: [],
			slug: null,
			loadingImage: require("../assets/loading.gif"),
		};
	},
	methods: {
		async fetchCovidData() {
			const res = await fetch("https://api.covid19api.com/summary");
			const data = await res.json();
			return data;
		},

		selectCountry(country) {
			if (!country) {
				this.clearCountry();
				return;
			}
			this.stats = country;
			this.title = country.Country;
			this.slug = country.Slug;
		},

		async clearCountry() {
			this.loading = true;
			const data = await this.fetchCovidData();
			this.stats = data.Global;
			this.title = "Global";
			this.loading = false;
			this.slug = null;
		},
	},

	computed: {
		formattedCountries() {
			return this.countries.map((country) => {
				return {
					...country,
					ID: country.ID,
					displayName: country.Country,
					value: country.ID,
				};
			});
		},
	},
	async mounted() {
		const { Date, Global, Countries } = await this.fetchCovidData();
		this.date = Date;
		this.stats = Global;
		this.countries = Countries;
		this.loading = false;
	},
};
</script>
