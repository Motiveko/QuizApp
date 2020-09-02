<template>
    <div class='popup_wrap'
        :class="numTotal>=10 ? 'showPopup' : ''"
    >
        <div class='popup_bg'></div>
        <div class='popup_cntnt'>
            <b-jumbotron>
                <template v-slot:header> 
                    {{numCorrect}}/{{numTotal}}
                </template>

                <template v-slot:lead>
                    {{ message }}
                    <!-- 결과값 : 모두 맞혔습니다! , 아깝습니다!, 문제가 어려웠나요?     -->
            
                </template>

                <hr class="my-4">

                <p> 다시 한번 도전하시겠습니까? </p>

                <b-button 
                    variant="primary"
                    @click="retry()"
                >Retry</b-button>
                <b-button variant="success" href="https://google.com">Exit</b-button>
            </b-jumbotron>
        </div>
     </div>   

</template>

<script>
export default {
    props: {
        numCorrect: Number,
        numTotal: Number
    },
    data() {
        return {
            message: ''
        }
        
    },
    watch: {
        numTotal: {
            handler(){
               if( this.numTotal >=10){
                   // 이렇게 안하면 transition안먹고 그냥들어간다. 왜그러는지는 모르겠으나.. 원리를 생각해서 뭐하리..
                   setTimeout( function(){
                       var e = document.querySelector('.popup_cntnt')
                       e.classList.add('show');
                   },1);

                    this.message = (this.numCorrect === this.numTotal) ? '축하드립니다!' :
                                     (this.numCorrect >= 5 ) ? '아쉽습니다!' : '문제가 많이 어려웠나요..?'
               }
                
            }
        }
    },
    methods: {
        retry(){
            location.reload();
        }
    }

    
}
</script>

<style scoped>

    .popup_wrap {
        display: none;
        position: absolute;
        top:0;
        left:0;
    }
    .popup_bg{
        position: absolute;
        height: 100vh;
        width: 100vw;
        background-color: gray;
        opacity: 0.3;
        /* display: flex;
        justify-content: center;
        align-items: center; */
    }
    .popup_cntnt{ 
        position: absolute;
        width: 600px;
        height: 100%; 
        background-color: white;
        
        top: calc(50vh - 250px);
        left: calc(50vw - 300px);
        opacity: 0;
    }
    .showPopup {
        display: block;
    }
    .show{
        
        opacity: 1;
        transition: 1.2s;
    }

</style>