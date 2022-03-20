<template>
	<div class="question-box-container">
		<b-jumbotron>
			<template #header>Enjoy Your Time!</template>

			<template #lead> {{ currentQuestion.question }} </template>

			<hr class="my-4" />

			<b-list-group>
				<b-list-group-item
					v-for="(answer, idx) in answers"
					:key="idx"
					@click="selectAnswer(idx)"
					:class="[idx === selectedIndex ? 'selected' : '']"
				>
					{{ answer }}
				</b-list-group-item>
			</b-list-group>

			<b-button
				variant="primary"
				@click="checkAnswer"
				:disabled="selectedIndex === null || answered"
				>Submit</b-button
			>
			<b-button
				variant="success"
				@click="next"
				:disabled="selectedIndex === null || !answered"
				>Next question</b-button
			>
		</b-jumbotron>
	</div>
</template>

<script>
import _ from "lodash";

export default {
	name: "QuestionBox",
	props: {
		currentQuestion: {
			type: Object,
		},
		next: {
			type: Function,
		},
		increment: {
			type: Function,
		},
	},
	data() {
		return {
			selectedIndex: null,
			answered: false,
		};
	},
	methods: {
		selectAnswer(idx) {
			console.log("selected", idx);
			this.selectedIndex = idx;
		},
		shuffleAnswers(answers) {
			return _.shuffle(answers);
		},
		checkAnswer() {
			this.answered = true;
			let isCorrect = false;

			if (this.correctAnswer === this.answers[this.selectedIndex]) {
				isCorrect = true;
			}
			this.increment(isCorrect);
		},
	},
	watch: {
		currentQuestion() {
			this.selectedIndex = null;
			this.answered = false;
		},
	},
	computed: {
		correctAnswer() {
			return this.currentQuestion.correct_answer;
		},
		answers() {
			let answers = [
				...this.currentQuestion.incorrect_answers,
				this.currentQuestion.correct_answer,
			];
			const shuffledAnswers = this.shuffleAnswers(answers);
			return shuffledAnswers;
		},
	},
};
</script>

<style scoped>
.question-box-container {
	text-align: center;
}

.list-group {
	margin-bottom: 10px;
}
.btn {
	margin-left: 5px;
	margin-right: 5px;
}

.list-group-item:hover {
	background: #eee;
}

.selected {
	background-color: lightgreen;
}

.correct {
	background-color: lightblue;
}

.incorrect {
	background-color: lightpink;
}
</style>
