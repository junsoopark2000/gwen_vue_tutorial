<template>
	<div>
		<b-nav>
			<b-nav-item disabled>Fancy Quiz App</b-nav-item>
			<b-nav-item disabled
				>Counter: {{ numCorrect }}/{{ numTotal }}
			</b-nav-item>
		</b-nav>
	</div>
</template>

<script>
export default {
	name: "TheHeader",
	components: {},
	props: {
		numCorrect: {
			type: Number,
		},
		numTotal: {
			type: Number,
		},
	},
	data() {
		return {};
	},
};
</script>
