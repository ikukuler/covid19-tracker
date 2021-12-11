<template>
	<label class="text-2xl block mt-10">Select Country</label>
</template>

<script>
import Selector from "@/components/Selector";
export default {
	name: "CountrySelect",
	props: ["countries"],
	components: {
		Selector,
	},
	data() {
		return {
			selected: 0,
		};
	},
	emits: ["selectCountry"],
	methods: {
		selectCountry(country) {
			this.$emit("selectCountry", country);
		},
	},
};
</script>
