<template>
	<div id="App">
		<TheHeader :numCorrect="numCorrect" :numTotal="numTotal" />

		<b-container class="bv-example-row">
			<b-row>
				<b-col>
					<QuestionBox
						v-if="questions.length"
						:currentQuestion="questions[index]"
						:next="next"
						:increment="increment"
					/>
				</b-col>
			</b-row>
		</b-container>
	</div>
</template>

<script>
import TheHeader from "@/components/TheHeader.vue";
import QuestionBox from "@/components/QuestionBox.vue";

export default {
	name: "App",
	components: {
		TheHeader,
		QuestionBox,
	},
	data() {
		return {
			questions: [],
			index: 0,
			numCorrect: 0,
			numTotal: 0,
		};
	},
	mounted() {
		fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
			method: "get",
		})
			.then((response) => {
				return response.json();
			})
			.then((jsonData) => {
				console.log(jsonData);
				this.questions = jsonData.results;
			});
	},
	methods: {
		next() {
			this.index++;
		},

		increment(isCorrect) {
			if (isCorrect) {
				this.numCorrect++;
			}
			this.numTotal++;
		},
	},
};
</script>

<style scoped></style>
