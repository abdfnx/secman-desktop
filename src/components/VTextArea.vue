<template>
	<div class="text-area-wrapper">
		<textarea
			:value="!sensitive ? value : _hidden"
			autocorrect="off"
			autocomplete="off"
			spellcheck="false"
			v-on="inputListeners"
			v-bind="$attrs"
			v-if="value != ''"
		/>
		<textarea
			v-else
			placeholder="No Extra"
			autocorrect="off"
			autocomplete="off"
			spellcheck="false"
			v-on="inputListeners"
			v-bind="$attrs"
		/>
		<!-- Error -->
		<p class="error" v-if="getError" v-text="getError" />
	</div>
</template>

<script>
export default {
	name: "VTextArea",

	props: {
		name: String,
		value: String,
		sensitive: Boolean,
	},

	computed: {
		getError() {
			const error = this.errors.items.find((e) => e.field == this.name);
			return error ? error.msg : "";
		},

		inputListeners() {
			return Object.assign({}, this.$listeners, {
				input: (event) => this.$emit("input", event.target.value),
			});
		},

		_hidden() {
			return "•".repeat(this.value.length);
			// if (this.value != "") {
			// } else {
			//   return "No Extra";
			// }
		},
	},
};
</script>

<style lang="scss">
.text-area-wrapper {
	display: flex;
	flex-direction: column;
	padding: $spacer-2 $spacer-3;
	width: 100%;

	textarea {
		width: 100%;
		resize: none;
		color: white;
		border-radius: 5px;
		padding: $spacer-2;
		min-height: 100px;
		background-color: $color-gray-500;
		border: solid 1px $color-gray-700;

		&::placeholder {
			font-weight: normal;
			font-size: $font-size-medium;
			color: $color-gray-300;
		}

		&:disabled {
			border: 0;
		}

		&:not(:placeholder-shown) {
			resize: vertical;
		}
	}

	.error {
		font-size: 10px;
		color: $color-danger;
		margin-top: $spacer-1;
	}
}
</style>
