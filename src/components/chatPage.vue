<template>
<div class="background">
  <div class="tatkoNaMafiu">
    <div class="headChat">
      <div class="topHead">
        <div class="chatIcon" id= "logo"></div>
      </div>
    </div>
    <div class="chatWrap">
      <div class="ChatWindow">
        <div class="singleMessageDiv"
        v-for="(msg, index) in message"
        v-bind:key="index"
        :class="[msg.myMsg ? 'my-message': '',msg.newUser ? 'new-user': '']"
        >
          <div class="wrapLine">
            <div class="Message">
              <div class="messageText" > {{msg.content}} </div>
              <div class="timeWrap">
                <div class="time"> {{msg.time}} </div>
              </div>
            </div>
          </div>
          </div>
        <div class="singleMessageDiv invis"></div>
        <div class="chat_window_space"></div>
        <div class="scrollDownButton" @click="scrollToEnd" v-if="!user.scrolled">
            <i class="material-icons">keyboard_arrow_down</i>
        </div>
      </div>
    </div>
    <div class="inputOMEGAWrap">
      <div class="inputWrap">
        <input  type="text" v-model="myMessage" @keyup.enter="submit" @keyup="showIcon" @focus="scrollToEnd">
        <div @click="submit" class="sendBtnWrap">
            <button @click="submit" class="btn_icon"></button>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import api from '../api';
import store from '../store.js';
import { mapState, mapActions } from 'vuex';
export default {
    data(){
        return{
            otherMessages: [],
            myMessage:'',
            message:[],
            userChat: {
              sid: null,
              name: null
            },
            logo_height: null
        }
    },
    name: 'chatPage',
    props: {
        msg: String
    },
    computed : {
        ...mapState(['messages','user','client'])
    },
    methods:{
        ...mapActions(['disconnectWS','connectToWS','subName','getClient']),
        scrollToEnd: function() {
            var container = this.$el.querySelector(".ChatWindow");
            // console.log(container);
            container.scrollTop = container.scrollHeight - 55;
            this.user.scrolled = true;
            this.resetSizes();
        },
        resetSizes(){
          let logo= document.getElementById('logo');
          logo.style.height= this.logo_height + 'px';
          logo.style.width= this.logo_height + 'px';
        },
        submit() {
            let time = new Date();
            let min = time.getMinutes();
            let hours = time.getHours();
            let name = this.user.name;
            this.message = this.messages;
            if(hours < 10){
                hours = "0" + hours;
            }
            if(min < 10){
                min = "0" + min;
            }

            let timeNow = hours + ":" + min;
            let currentMessage = this.myMessage;

            if(currentMessage == '')
                return;

            let newMsg = {
                "content":currentMessage,
                "time": name + " " + timeNow,
                "sid": this.user.sid,
                "name": "Logged"
            }
            // console.log(this.messages);
            // this.messages.push(newMsg);
            this.user.ws.send(JSON.stringify(newMsg));
            // console.log('sent');
            this.scrollToEnd();
            this.myMessage = '';
        },
        userJoined() {
            let time = new Date();
            let min = time.getMinutes();
            let hours = time.getHours();

            if(hours < 10){
                hours = "0" + hours;
            }
            if(min < 10){
                min = "0" + min;
            }
            let timeNow = hours + ":" + min;
            let currentMessage = this.myMessage;

            let name = this.user.name;
            let newMsg = {
                "content": "~~~" + name + " has joined room~~~",
                "time": timeNow,
                "sid": this.user.sid,
                "name": name
            }
            this.user.ws.send(JSON.stringify(newMsg));
        },
        showIcon(){
          let icon = this.$el.querySelector('.btn_icon');
          let inputValue = this.myMessage;
          if(inputValue !== ''){
            icon.style.display = 'block';
          }else{
            icon.style.display = 'none';
          }
        },
        insertDocument(){
          if (this.windowSize()) {
            let insertDocument = document.querySelector('.contentWraper');
            let content = document.querySelector('.content');
            files.style.display = 'block';
            window.addEventListener('click', (e) => {
              if (e.target != content) {
                files.style.display = 'none';
              };
            });
          }else{
            this.chooseDocument();
          }
        },

        getContent(){
          var constraints = { audio: false, video: { width: 1280, height: 720 } };
          navigator.mediaDevices.getUserMedia(constraints)
            .then(function(mediaStream) {
              var videoWrapper = document.querySelector('.videoWrapper');
              videoWrapper.style.display = 'block';
              var video = document.querySelector('.video');
              video.srcObject = mediaStream;
              video.onloadedmetadata = function(e) {
                video.play();
              };
            }).catch(function(err) { console.log(err.name + ": " + err.message); });
            // TO DO: finish with the camera thing
        },
        chooseDocument(){

           let inputDocument = document.querySelector('[name="document"]');
           let image = document.querySelector('.image');
           inputDocument.click();
           inputDocument.addEventListener('change', (event) => {
             let inputValue = event.target.files[0];
             //need to save image so we can send it;
           });
        },
        windowSize(){
          if (window.matchMedia("(max-width: 1025px)").matches) {
            return true;
          }else{
            return false;
          }
        }
    },
    created () {
      this.windowSize();
    },
    mounted () {
      this.logo_height = document.getElementById('logo').clientHeight;
      console.log(this.logo_height)
      console.log('xddd')
      this.message = this.addedMessages;
      if(this.user.name != null){
        this.userJoined(); 
      }else{
        let namee = window.localStorage.getItem('name')
        console.log('userChat nameee: ',namee)
        let response = this.subName(namee)
        if(response == 'nema ime' || response == 'vrati na popup page')
        router.push('/')
      }
      setTimeout(() => {
        console.log(this.user.sid)
        this.getClient();
        this.submit();
      }, 1000);
    },
    beforeDestroy(){
      this.disconnectWS();
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.chat_window_space{
  width: 100%;
  height: 14px;
}

.chatTop{
  height: 10vh;
  min-height: 60px;
  width: 100%;
}
.scrollDownButton {
    animation: MoveUpDown 1s ease-in-out infinite;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    position: fixed;
    transform: translateX(-50%);
    right: 10px;
    bottom: 500px;
    border-radius: 50%;
    background-color: rgba(121, 121, 121, 0.65);
    border: 1px rgb(58, 58, 58);
}
@keyframes MoveUpDown {
    0%, 100% {
        bottom: 10vh;
    }
    50% {
        bottom: 11vh;
    }
}
div.singleMessageDiv.my-message{
    justify-content: flex-end;
    margin-right: 25vw;
}
.invis{
    visibility: hidden;
    height: 50px;
}
.singleMessageDiv{
    display: flex;
    justify-content: flex-start;
    margin-left: 25vw;
}
div.singleMessageDiv.my-message .wrapLine .Message{
    background-color:rgb(255, 248, 248);
}
div.singleMessageDiv.new-user .wrapLine .Message{
    background-color: rgba(0, 253, 34, 0);
    color:rgb(255, 255, 255);
    box-shadow: none;
    font-size: 15px;
    border-width: 0;
}
div.singleMessageDiv.new-user .wrapLine .timeWrap{
    color:white;
}

div.singleMessageDiv.new-user{
    width: 50vw;
    margin-left: 25vw;
    justify-content: center;
    background-color: rgba(0, 253, 34, 0.267);
}

div.singleMessageDiv.new-user .timeWrap{
    justify-content: center;
}
body{
    display: flex;
    justify-content: center;
}
.tatkoNaMafiu{
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    margin: 0;
    padding: 0;
    border-width: 0;
}
.chatWrap{
    width: 100%;
}
.chatIcon{
    
    width: 10vh;
    height: 10vh;
    min-height: 50px;
    min-width: 50px;
    margin: 0 auto;
    background-size: cover;
    background-position: center center;
    background-repeat: no-repeat;
    background-image: url('./../assets/logo.png');
}


.headChat{
    position: fixed;
    z-index: 1;
    border-style:solid;
    display: flex;
    flex-direction: column;
    width: 50vw;
    height: 10vh;
    min-height: 60px;
    background-color: rgb(65, 62, 62);
    border-color: rgb(156, 173, 1);
    border-width: 2px;
    margin-left: 25vw;
    margin-bottom: 10vw;
}

.topHead{
    display: flex;
    margin: 0 auto;
}
.botHead{
    margin: 0 auto;
    color: rgb(48, 48, 48);
    font-size: 10px;
    margin: 0 0 5px 0;
}
.roomName{
    font-size: 10px;
    color: #f8c262;
    font-size: 20px;
    margin: 17px 0 5px 0px;
}
.roomWrap{
    width: 100%;
    display: flex;
    align-items: flex-start;
    flex-direction: column;
    margin-left: 10px;
}
.inputOMEGAWrap{
    position: fixed;
    bottom: 0;
    margin-bottom: 2vh;
    width: 50vw;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 25vw;
}
.inputWrap{
    background-color: rgba(87, 80, 80, 0.829);
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 10px;
    /* height: 5vh; */
    width: 90vw;
    border-width: 1px;
    border-style: solid;
    border-color: #f8c262;
}
.inputWrap input{
    display: block;
    font-size: 18px;
    width: calc(100% - 50px);
    height: 5vh;
    padding: 0 5px;
    font-size: 14px;
    border-width: 0;
    background-color: rgba(87, 80, 80, 0);
    color: #f8c262;
}

.sendBtnWrap{
    width: 35px;
    height: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.Message{
  min-width: 120px;
  height: auto;
  min-height: 20px;
  background-color: #F7E7CE;
  float: right;
  margin: 7px 20px;
  font-size: 19px;
  font-weight: bolder;
  border-radius: 15px;
  color: rgb(0, 0, 0);
  font-weight: 300;
  word-break: break-all;
  display: flex;
  flex-direction: column;
  box-shadow: 1px 1px;
}
.messageText{
  text-align: left;
  margin:3px 5px;
}
.wrapLine{
  display:flex;
  max-width: 500px;
  height: fit-content;
  flex-wrap:  wrap;
  justify-content:flex-end;
  margin-right: 0;
}
.time{
  font-size: 10px;
  margin-left: 5px;
  font-weight: bold;
}
.timeWrap{
  color: rgb(78, 78, 78);
  background-color: rgba(255, 255, 255, 0);
  display:flex;
  justify-content: flex-end;
  margin: 0 6px 4px 0 ;
}
.chatWrap{
  align-content: center;
  display: inline-block;
  color: white;
  /* height: 81.5vh; */
  margin-top: 10vh;
}
.ChatWindow{
  position: relative;
  width: 100%;
  height: 82vh;
  border-width: 0px;
  border-style:solid;
  overflow-y: scroll;
  display: flexbox;
  scrollbar-arrow-color: black;
  /* flex-direction: column; */
}
.ChatWindow::-webkit-scrollbar-track{
  background-color: black;
  margin-right: 20vw;
}
.chatWrap::-webkit-scrollbar-track{
  background-color: black;
  margin-right: 20vw;
}

.inputWrap button{
    /* display: none; */
    width: 29px;
    height: 29px;
    color: gray;
    background-color: rgb(0, 0, 0);
    color: rgb(97, 97, 97);
    font-weight: bolder;
    font-family: sans-serif;
    font-size: 18px;
    float: right;
    border: 2px solid gray;
    border-radius: 30px;
    margin: 4px;
    background-image: url('../assets/arrow-up.png');
    background-size: 30px 30px;
    background-color: #fff;
    background-repeat: no-repeat;
    background-position: center center;
    overflow: hidden;
}
.content{
  background-image: url('../assets/clip.png');
  background-color: #F7E7CE;
  background-size: 25px 25px;
  background-repeat: no-repeat;
  background-position: center center;
  cursor: pointer;
  margin: 4px;
  width: 25px;
  height: 25px;
  display: block;
  float: left;
}
.secret{
  display: none !important;
}
.contentWraper{
  display: none;
  position: absolute;
  bottom: 7%;
  left: 1%;
  width: 150px;
  text-align: center;
}
.contentWraper ul{
  list-style: none;
  padding: 0;
  margin: 0;
}
.contentWraper ul li{
  border-bottom: 1px solid gray;
  padding: 15px;
  background-color: #fff;
  padding-bottom: 5px;
  font-family: 'Roboto', sans-serif;
  font-weight: bolder;
  color: rgba(7, 43, 8, 0.979);
  cursor: pointer;
}
.contentWraper ul li:hover{
  background-color: rgba(7, 43, 8, 0.979);
  color: #fff;
}
.triangle {
  margin: 0 auto;
  width: 0;
  height: 0;
  border-left: 75px solid transparent;
  border-right: 75px solid transparent;
  border-top: 50px solid #fff;
}
.videoWrapper{
  position: absolute;
  top: 10%;
  left: 10%;
}
/*------------------------------------------------------------------------------*/
.background{
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

@media screen and (max-width: 720px){
.background{
   background-image: none;
   background-color: rgb(34, 27, 27);
}
div.singleMessageDiv.new-user{
    width: 95vw;
    margin: 2.5vw;
}
div.singleMessageDiv.my-message{
    margin-right: 0;
}
.singleMessageDiv{
    margin-left: 0;
}
.inputOMEGAWrap{
    width: 80vw;
    margin-left: 10vw;
}
.headChat{
  width: 99%;
  margin-left: 0;
}

}


</style>
