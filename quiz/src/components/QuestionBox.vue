<template>
    <div class="question-box-container">
        <b-jumbotron>

            <template v-slot:lead>
                {{ currentQuestion.question}}
            </template>

            <hr class="my-4">

            <!-- (answer, index)는 데이터와 index를 추출하게 해준다. :key는 해당 값에 explicit key를 할당하는것. 안하면 뭐라한다. -->
           <!-- :class는 조건이 복잡하고 이를 methods에 정의한다. -->
            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index" 
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                
                </b-list-group-item>

            </b-list-group>

 

            <b-button 
                variant="primary"
                v-on:click="submitAnswer"    
                :disabled="selectedIndex === null || answerd"    
            >Submit</b-button>
            <b-button @click="next" variant="success" >
                Next
            </b-button>

        </b-jumbotron>
    </div>    
</template>

<script>
// _.~~ 로 lodash를 쓸 수 있다.
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function, // tpye지정, 부모컴포넌트에서 v-bind로 보내주는걸 받을 때 type이다.
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answerd: false,
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]; // array에 복사
            answers.push(this.currentQuestion.correct_answer);
            return answers
        }
    },
    watch: {

        // currentQuestion이 바뀌면 이함수가 실행된다. 근데 맨 처음에도 실행해야한다.
        // currentQuestion(){
        //    this.selectedIndex = null;
        //    this.shuffleAnswers(); 
        // }

        // 해결 ::
        currentQuestion: {
            immediate: true, // 맨첨에 한번 실행한다.
            handler(){
                this.selectedIndex = null;
                this.answerd = false;
                this.shuffleAnswers(); 
            }
        }
    
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index;
        },
        submitAnswer(){
            let isCorrect = false;
            if( this.selectedIndex===this.correctIndex){
                isCorrect = true;
            }
            this.increment(isCorrect);
            this.answerd = true;
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            // lodash :: javascript library with very useful utilsnp
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        answerClass(index){
            let answerClass ='';
            if(!this.answerd && this.selectedIndex === index){
                answerClass = 'selected';
            } else if(this.answerd && this.correctIndex === index){
                answerClass = 'correct';
            } else if(this.answerd && this.selectedIndex === index && this.correctIndex ){
                answerClass= 'incorrect';
            }
            return answerClass;
        }
    }
}
</script>


<style scope>
/* scope : 이 component안에서만 먹게하겠다. 클래스명은 브라우저에서 개발자도구로 확인해서 가져와야한다. */
.list-group{
    padding-bottom: 15px;
}
.list-group-item:hover {
    background-color: #EEE;
    cursor: pointer;
}
.btn {
    margin: 0 5px;
}
.selected {
    background-color: cornflowerblue !important;
}
.correct {
    background-color: lightgreen !important;
}
.incorrect {
    background-color: red !important;
}
</style>