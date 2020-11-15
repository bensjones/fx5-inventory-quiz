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
    export default {
        name: 'quizEngine',
        data() {
            return {
                questions: [{
                    question_description: "what is 3 + 4",
                    correct_answer: 3,
                    wrong_answer_response: "it is 7",
                    answer_option_1: "3",
                    answer_option_2: "2",
                    answer_option_3: "36",
                    answer_option_4: "7",
                },
                {
                    question_description: "what is 6 + 2",
                    correct_answer: 3,
                    wrong_answer_response: "it is 8",
                    answer_option_1: "3",
                    answer_option_2: "2",
                    answer_option_3: "36",
                    answer_option_4: "8",
                }],
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
        created() {
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