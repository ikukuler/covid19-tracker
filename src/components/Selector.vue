<template>
	<select
		v-model="selected"
		class="form-select rounded mt-4 mb-5 block p-1 w-full border"
		@change="onChange"
	>
		<option value="0">Select {{ name }}</option>
		<option v-for="item in items" :key="item.ID" :value="item.value">
			{{ item.displayName }}
		</option>
	</select>
</template>

<script>
export default {
	name: "Selector",
	props: ["items", "name", "default"],
	data() {
		return {
			selected: 0,
		};
	},
	emits: ["select"],
	methods: {
		onChange() {
			const item = this.items.find((item) => item.ID === this.selected);
			this.$emit("select", item);
		},
	},
};
</script>
