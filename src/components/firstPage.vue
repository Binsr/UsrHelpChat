
<template>
  <div class="Container">
        <div class="terms-of-use">
          <p class="welcome_para">Welcome, {{" " +this.user.name}} please accept terms of use </p>
          <div class="term-text">
            <p>Some shit people need to accept</p>
          </div>
          <div class="chatBtnContainer">
            <button class="chatBtn" @click="toChatPage"><p class="btParagraf" id="clickme">Accept</p></button>
          </div>
        </div>
  </div>
</template>

<script>
import router from "../router.js"
import api from '../api'
import { mapState, mapActions } from "vuex"
import cafeList from './cafeList.vue'
import store from '../store'

export default {
    components: {
      cafeList
    },
    data() {
      return{
      }
    },
    computed : {
        ...mapState(['messages','user','client'])
    },
    methods: {
      ...mapActions(['addUsername','client']),
        showCaffes(){
          if(this.caffeApi){
            api.getCaffeNames().then(Response => {
              this.caffeNames = Response.data.caffes;
              // console.log(this.caffeNames);
            });
            this.caffeApi = false;
          }
          this.showCaffeList = true;
        },
        toChatPage () {
          clearInterval(this.interval);
          if(this.user.ip != "NOT_CONNECTED")
            router.push('/chatpage');
          else{
            this.typing();
          }
        },
        imagesBuild(img){
          return ('https://' + img + '.jpg');
        },
    },
    mounted() {
          // this.typing();
    },
    computed: {
      ...mapState(['user']),
    },
    beforeDestroy() {
      clearInterval(this.interval);
    }
}
</script>

<style>


.terms-of-use{
  display: flex;
  width: 60%;
  max-width: 500px;
  min-width: 300px;
  min-height: 300px;
  margin: 0 auto;
  margin-top: 10%;
  flex-direction: column;
  background-color: #000000d0;
  box-shadow: 1px 1px rgb(100, 100, 100);
  border-width: 1px;
  border-style: solid;
  border-color: #f8c262;
}

.welcome_para{
  color: white;
}

.term-text{
  width: 80%;
  max-width: 400px;
  min-width: 200px;
  min-height: 200px;
  margin: 0 auto;
  margin-top: 5%;
  background-color:rgba(255, 255, 255, 0.349);
  margin: 0 auto;
}

.Container{
  width: 100vw;
  height: 100vh;
  background-color: white;
  border-style: solid; 
  border-color: white;
  background-image: url('./../assets/bak6.jpg');
  background-size: 100%;
  background-repeat: no-repeat;
  background-position: center;
  overflow:hidden;
  border-width: 0;
  display: block;
  height: 100%;
  width: 100vw;
  background-position: center center;
  background-repeat: no-repeat;
  background-size: cover;
}

.chatBtn {
  width: 17%;
  height: 10%;
  min-width: 80px;
  min-height: 40px;
  color: #f8c262;
  margin: 0 auto;
  background-color:rgba(255, 255, 255, 0.349);
  border-radius: 5px;
  border-width: 0;
}

.chatBtnContainer{
  display: flex;
  width:100%;
  margin: 3px 0 3px 0;
}

.chatBtnContainer :hover{
  background-color: rgb(155, 129, 129);
}

@media screen and (max-width: 720px){
:root{
    font-size: 12px;
  }
.terms-of-use{
        width: 98%;
        height: 50%;
    }
}


</style>
