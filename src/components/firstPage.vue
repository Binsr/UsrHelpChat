
<template>
  <div class="Container">
        <div class="terms-of-use">
          <p>Terms of use</p>
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

export default {
    components: {
      cafeList
    },
    data() {
      return{
      }
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
  background-color: rgba(59, 64, 80, 0.514);
  display: flex;
  width: 40%;
  min-width: 300px;
  min-height: 300px;
  margin: 0 auto;
  margin-top: 10%;
  flex-direction: column;
  box-shadow: 1px 1px rgb(100, 100, 100);
  border-width: 1px;
  border-style: solid;
  border-color: #f8c262;
  position: relative;
}

.term-text{
  width: 400px;
  height: 200px;
  background-color:rgba(255, 255, 255, 0.349);
  margin: 0 auto;
}

.Container{
    width: 100vw;
    height: 100vh;
    background-color: white;
    background-image: url('./../assets/bak6.jpg');
    background-size: 100%;
    background-repeat: no-repeat;
    background-position: center;
    overflow:hidden;
    padding: 0;
    border-width: 0;
    
}

.chatBtn {
  width: 100px;
  height: 40px;
  color: #f8c262;
  margin-left: 8px;
  margin-bottom: 5px;
  background-color:rgba(255, 255, 255, 0.349);
  border-radius: 5px;
  position: absolute;
  bottom: 0;
  border-width: 0;
}
.chatBtnContainer{
  display: flex;
}
.chatBtnContainer :hover{
  background-color: rgb(0, 0, 0);
}



</style>
