<template>
    <div>
        <h1>Welcome to Five X 5 Inventory 101 Quiz</h1>
        <div class="left">
            <button @click="startQuiz">Take Quiz</button>
        </div>
        <div id="quiz" v-show="testing">
            <quiz-engine />
        </div>
    </div>
</template>

<script>
import QuizEngine from './../components/QuizEngine.vue'
    export default {
      components: { QuizEngine },
            name: 'Dashboard',
            data() {
                return{
                    history: [],
                    testing: false
                }
            },
            methods: {
                startQuiz() {
                    this.testing = !this.testing
                }
            }
        }
</script>

<style lang="scss" scoped>

</style>