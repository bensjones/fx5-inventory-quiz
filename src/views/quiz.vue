<template>
  <div class="quiz">
    <quiz-engine />
  </div>
</template>

<script>

import QuizEngine from '@/components/QuizEngine.vue';

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