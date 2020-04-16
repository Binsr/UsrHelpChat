<template>
    <div class="popupContainer">
        <div class="sheepLogo">
          <img src="../assets/mainlogo.jpg"/>
        </div>
        <div class="title">
            <div class="titleFSpart">Live help center</div>
          <div class="areaPart">App</div>
        </div>
      <div>
          <div class="inputContainer">
            <div class="input">
                    <input type="text" v-model="name" @keyup.enter="submitName" placeholder="~~~Ime~~~">
            </div>
            <div class="btnOkContainer">
                <button @click="submitName">Login</button>
            </div>
          </div>
      </div>
    </div>
</template>
<script>
import {mapActions, mapState} from 'vuex';
import router from '../router';
import api from '../api'
import { setTimeout, clearInterval } from 'timers';

export default {
    data(){
        return {
            name: '',
            typingAllowed: true
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
    }
}
</script>

<style>
.popupContainer{
    width: 100vw;
    height: 100vh;
    background-color: white;
    border-style: solid; 
    border-color: white;
}
.sheepLogo{
    margin: 0 auto;
    width: 100vw;
    background-color: rgb(179, 77, 29);
}
.sheepLogo img{
    margin: 0 auto;
    border-width: 20px;
    border-style: solid;
    border-color: black;
    border-radius: 50%;
}

.titleFSpart{
  color: rgb(167, 167, 167);
  text-shadow: 2px 2px #fff;
}

/* TITLE EDIT PART
------------------------------------*/

.title {
    display: flex;
    text-align: center;
    font-size: 10vw;
    margin: 0 auto;
    flex-direction: column;
}

.titleFSpart {
  color: rgb(121, 121, 121);
  letter-spacing: 3px;
  font-family: "Bangers", cursive;
  text-shadow: 2px 2px black;
}
.areaPart {
  color: rgb(255, 255, 255);
  text-shadow: 2px 2px rgb(255, 255, 255);
}

.wifiContainer{
    display: flex;
    width: 100vw;
}
.writingTextContainter{
    transform: rotate(10deg);
    color:white;
}

/* ----------------------------------*/
.inputContainer{
    display:flex;
    width: 100vw;
    position: absolute;
    bottom: 0%;
    flex-direction: column;
    margin-bottom: 2vh;
}
.input{
    margin: 0 auto;
}
.input input{
    width: 40vw;
    height: 6vh;
    background-color: rgb(158, 147, 147);
    border-radius: 10px;
    text-align: center;   
    color: white;
}
.input ::placeholder{
    color: white;
}
.input input:focus::placeholder{
    color:black;
}
.input input:focus{
    background-color: rgb(95, 48, 48);
}
.btnOkContainer{
    width: 100vw;
    display: flex;
    align-content: center;
    margin-top: 5vh;
}
.btnOkContainer button{
    margin: 0 auto;
    color: rgb(184, 0, 0);
    border: none;
    outline: none;
    display: flex;
    background-color: rgb(255, 255, 255);
    font-size: 10vw;
    font-weight: bold;
    border-radius: 20px;
    cursor: pointer;
}

</style>
