<template>
    <div>
        <h1>Five X 5 Inventory 101 Quiz</h1>
        <h2>Question {{currentIndex + 1}} of {{questions.length}}</h2>
        
        <div class="d-flex flex-columm justify-content-center">
            <b-card
                v-show="!isFinished"
            >
                <b-card-text>
                    <h3 class="question-description">{{currentQuestion.question_description}}</h3>
                    <b-button-group vertical size="lg" class="mt-3">
                        <b-button block variant="outline-dark" @click="selectAnswer(0)">{{currentQuestion.answer_option_1}}</b-button>
                        <b-button block variant="outline-dark" @click="selectAnswer(1)">{{currentQuestion.answer_option_2}}</b-button>
                        <b-button block variant="outline-dark" @click="selectAnswer(2)">{{currentQuestion.answer_option_3}}</b-button>
                        <b-button block variant="outline-dark" @click="selectAnswer(3)">{{currentQuestion.answer_option_4}}</b-button>
                    </b-button-group>
                </b-card-text>
            </b-card>

            <b-card
                v-show="isFinished"
            >
                <b-card-text>
                    <h3 class="question-description">Way to go!</h3>
                    <p> You got <strong>{{correctCount}}</strong> out of <strong>{{questions.length}}</strong> correct! </p>
                </b-card-text>
            </b-card>
        </div>

        <div class="check-response-bar mt-3" v-show="!isFinished">
            <b-alert :show="isCheckingAnswer && isCorrect" variant="success">Correct! You got it!</b-alert>
            <b-alert :show="isCheckingAnswer && !isCorrect" variant="danger">{{currentQuestion.wrong_answer_response}}</b-alert>
            <b-button block variant="primary" v-show="showCheckButton" @click="checkAnswer">Check Answer</b-button>
            <b-button block variant="primary" v-show="showNextButton" @click="nextQuestion">Next Question</b-button>
            <b-button block variant="primary" v-show="showFinishButton" @click="showResults">Finish</b-button>
        </div>
        <div class="mt-3" v-show="isFinished">
            <b-button block variant="primary" @click="restartQuiz()">Take Again</b-button>
        </div>
    </div>
</template>

<script>
    import * as axios from 'axios';
    export default {
        name: 'quizEngine',
        data() {
            return {
                questions: undefined,
                currentIndex: 0,
                currentQuestion: undefined,
                showCheckButton: false,
                isCheckingAnswer: false,
                isCorrect: false,
                showNextButton: false,
                showFinishButton: false,
                isFinished: false,
                correctCount: 0,
                results: {}
            }
        },
        async created() {
            // this.questions = this.getQuestionsFromAPI();
            this.questions = this.getMockQuestions();
            this.currentQuestion = this.questions[this.currentIndex];
        },
        methods: {
            checkAnswer() {
                this.showCheckButton = false;
                this.isCheckingAnswer = true;
                if(this.currentIndex+1 === this.questions.length) {
                    this.showFinishButton = true;
                } else {
                    this.showNextButton = true;
                }
            },
            async getQuestionsFromAPI() {
                const api_url = 'http://localhost:8080/api/questions/'
                const response = await axios.get(api_url);
                this.questions = response.data;
            },
            getMockQuestions() {
                return [{
                    question_description: "Which one of these is NOT considered inventory?",
                    correct_answer: 3,
                    wrong_answer_response: "This is considered inventory.",
                    answer_option_1: "Liquor Bottles",
                    answer_option_2: "Fermentables/non-alcoholic material",
                    answer_option_3: "Beer, Wine, Cider",
                    answer_option_4: "Scratch and sniff stickers",
                },
                {
                    question_description: "What is a Bulk Distilled Spirit?",
                    correct_answer: 1,
                    wrong_answer_response: "A distilled spirits in a container having a capacity in excess of one wine gallon",
                    answer_option_1: "A distilled spirits in a package having a capacity in excess of 10 wine gallons",
                    answer_option_2: "A distilled spirits in a container having a capacity in excess of one wine gallon",
                    answer_option_3: "A package of distilled containers",
                    answer_option_4: "All of your ghosts friends in the attic.",
                }]
            },
            selectAnswer(index) {
                this.isCorrect = index === this.currentQuestion.correct_answer;
                if(this.isCorrect) this.correctCount++;
                this.showCheckButton = true;
            },
            nextQuestion() {
                this.isCheckingAnswer = false;
                this.showNextButton = false;
                this.currentIndex++;
                this.currentQuestion = this.questions[this.currentIndex];
            },
            showResults() {
                this.isFinished = true;
            },
            restartQuiz() {
                this.currentIndex = 0;
                this.correctCount = 0;
                this.currentQuestion = this.questions[this.currentIndex];
                this.isCheckingAnswer = false;
                this.showFinishButton = false;
                this.showCheckButton = false;
                this.showNextButton = false;
                this.isFinished = false;
            }
        }
    }
</script>

<style lang="scss" scoped>
    .btn-group-vertical > button{
        margin-bottom:10px;
        border-radius:10px !important;
    }
</style>