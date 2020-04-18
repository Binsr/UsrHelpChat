<template>
    <div class="popupContainer" id="container" @mouseover="updateElts">
        <div class="log-container" id="log-container">
            <div class="title">
                <div class="titleFSpart">Live info chat</div>
            </div>
            <div class="note">Some text about company!</div>
            <div class="inputContainer">
            
                <div class="input">
                    <input id="name_input" type="text" v-model="name" @keyup.enter="submitName" @focus="mobileShow" placeholder="Enter your name">
                </div>
            
                <div class="btnOkContainer" id="logbtn">
                    <button id="login_button" @click="submitName">
                        <div class="login" id="login_div">Login</div>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import {mapActions, mapState} from 'vuex';
import router from '../router';
import api from '../api';
import VueTouch from 'vue-touch';
import { setTimeout, clearInterval } from 'timers';


export default {
    data(){
        return {
            name: '',
            typingAllowed: true,
            is_keyboard: false,
            initial_screen_size: null,
            input_height: null,
            login_button_height: null,
            login_div_height: null
        }
    },
    computed:{
    ...mapState(['user','client']),
    },
    methods:{
        ...mapActions(['addUsername','messages','subName']),
             submitName(){
                 window.localStorage.setItem('name',this.name);
                let response = this.subName(this.name);
                if(response == 'nema ime'){
                    this.typing("Morate uneti ime prvo");
                    this.typingAllowed = false;
                }
                router.push('/firstpage')
            },
            isPcF(){
                this.isPc= true;
                console.log("aaaaa");
            },
            updateElts(){
                let log = document.getElementById('login_div');
                let height = log.clientHeight;
                log.style.lineHeight = height + 'px';
            },
            mobileShow(){
                console.log("Promenio");
                let ni = document.getElementById("name_input") 
                let stri= this.input_height + "px";
                document.getElementById('login_div').style.height = this.login_div_height + 'px';
                document.getElementById('login_button').style.height= this.login_button_height + 'px';
                ni.style.height= stri;
            },
            typing(poruka) {
                let text = poruka;
                const el = document.querySelector("#poptext");
                if(poruka){
                    el.textContent = "";
                }

                var i = 0;
                this.interval = setInterval(function () {
                    if (i < text.length) {
                        el.textContent += text[i];

                        if (i == text.length) {
                            return;
                        }
                    }
                    i++;
                }, 90);
            },
        },
    mounted(){
        console.log(this.initial_screen_size);
        this.input_height = document.getElementById('name_input').clientHeight;
        this.initial_screen_size = document.getElementById('log-container').clientHeight;
        this.login_button_height = document.getElementById('login_button').clientHeight;
        this.login_div_height = document.getElementById('login_div').clientHeight;
        this.updateElts();
    }
}
</script>

<style>
body{
    margin: 0;
    padding: 0;
}

html{
    margin: 0;
    padding: 0
}
.logo{
    background-image: url('./../assets/logo.png');
    background-size: contain;
    background-repeat: no-repeat;
    width:100px;
    height: 100px;
}

.popupContainer{
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
.log-container{
    display: flex;
    width: 60%;
    max-width: 500px;
    min-width: 300px;
    margin: 0 auto;
    margin-top: 10%;
    flex-direction: column;
    background-color: #000000d0;
    box-shadow: 1px 1px rgb(100, 100, 100);
    border-width: 1px;
    border-style: solid;
    border-color: #f8c262;
}


/* TITLE EDIT PART
------------------------------------*/

.title {
    display: flex;
    text-align: center;
    font-size: 3rem;
    margin: 0 auto;
    flex-direction: column;
    margin-top: 3vh;
    color:#f8c262;
}

/* ----------------------------------*/
.inputContainer{
    margin-top: 2vh;
    display:flex;
    flex-direction: column;
    margin-bottom: 2vh;
}
.input{
    margin: 0 auto;
    margin-top: 4vh;
}
.input input{
    width: 30vw;
    min-width: 200px;
    max-width: 320px;
    height: 6vh;
    max-height: 40px;
    min-height: 30px;
    background-color: rgba(0, 0, 0, 0.288);
    border-radius: 5px;
    text-align: center;   
    color: rgb(3, 3, 3);
    border-color: #f8c262;
    font-size: 0.8rem;
}

.input input::placeholder{
    color: white;
}

.input ::placeholder{
    color: rgb(12, 12, 12);
}
.input input:focus::placeholder{
    color:black;
}
.input input:focus{
    background-color: rgba(150, 132, 33, 0.397);
}
.btnOkContainer{
    display: flex;
    align-content: center;
    margin-top: 4vh;
    text-align: center;
    max-height: 40px;
    min-height: 30px;
}

.btnOkContainer button:hover{
    background-color: rgb(187, 187, 187);
}

.btnOkContainer button{
    /* padding-left: 12%; */

    text-align: center; 
    color: rgb(36, 35, 35);
    margin: 0 auto;
    border: none;
    outline: none;
    display: flex;
    background-color: rgba(155, 155, 155, 0.336);
    border-radius: 4px;
    cursor: pointer;
    font-size: 1.5rem;
}

.login{
    width: 30vw;
    height: 6vh;
    margin: auto auto;
    max-height: 40px;
    min-height: 30px;
    min-width: 180px;
    max-width: 300px;
}
.note{
    font-size: 0.9rem;
    display: flex;
    margin: 0 auto;
    margin-top: 4vh;
    color: #949494;
}


@media screen and (max-width: 500px){
:root{
    font-size: 6px;
    }

}
@media screen and (max-width: 570px){
  :root{
    font-size: 7px;
  }
}
@media screen and (max-width: 620px){
:root{
    font-size: 8px;
  }
}
@media screen and (max-width: 680px){
:root{
    font-size: 9px;
}
}
@media screen and (max-width: 720px){
:root{
    font-size: 12px;
  }
.log-container{
        width: 98%;
        height: 60%;
    }
}
@media screen and (max-width: 800px){
:root{
    font-size: 11px;
  }
}
@media screen and (max-width: 860px){
:root{
    font-size: 12px;
  }
}
@media screen and (max-width: 920px){
  html{
    font-size: 13px;
  }
}
@media screen and (max-width: 1000px){
:root{
    font-size: 14px;
  }
  
}


</style>
