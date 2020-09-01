<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
     />


      <b-container class="bv-example-row">
        <b-row>
            <!-- 6은 내가 차지하는거, offset은 왼쪽에 기본으로 넣는 여백(시작지점), 
                  기본적으로 grid는 12개의 컬럼을 가지고 있다 아렇게하면 딱중간 -->
          <b-col sm="6" offset="3">
            <!-- fetch가 완료되어 questions에 값이 있으면 binding한다. 
              이렇게 안하면 QuestionBox에서는 해당내용을 undefined로 인식한다. 어렵네
              v-bind는 data가 바뀐다고 계속 넘어가는 형태가 아닌가보다. 
            -->
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
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      //ES6문법 <- next: function(){}  
      this.index++;
    },
    increment(isCorrect){
      if(isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted: function(){
    //
    fetch('https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple', {
        method: 'get'
      }).then( (response) => {
        // no-console error ::: package.json - rules에 "no-console": 0 추가
        // console.log(response.json());
        return response.json();
      }).then((jsonData)=>{
        // vue-dev-tools chorome plugin을 설치하면 굳이 콘솔로 안찍어도 vue의 데이터를 확인해 볼 수 있다.
        this.questions = jsonData.results;
      });
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
