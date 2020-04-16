<template>
    <div class="popupContainer">
        <div class="log-container">
            <div class="title">
                <div class="titleFSpart">Live info chat</div>
            </div>
            <div class="note">There will always be someone online to answer your questions!</div>
            <div class="inputContainer">
                <div class="input">
                    <input type="text" v-model="name" @keyup.enter="submitName" placeholder="Enter your name please">
                </div>
                <div class="btnOkContainer">
                    <button @click="submitName">
                        <div class="login" >Login</div>
                    </button>
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
body{
    margin: 0;
    padding: 0;
}

html{
    margin: 0;
    padding: 0
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
}
.log-container{
    display: flex;
    width: 40%;
    min-width: 300px;
    margin: 0 auto;
    margin-top: 13%;
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
    font-size: 6vw;
    margin: 0 auto;
    flex-direction: column;
    margin-top: 3vh;
    color:#f8c262;
}

.titleFSpart {
  letter-spacing: 3px;
  font-size: 40px;
}
.areaPart {
  color: rgb(255, 255, 255);
  text-shadow: 2px 2px rgb(255, 255, 255);
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
    height: 6vh;
    background-color: rgba(0, 0, 0, 0.288);
    border-radius: 5px;
    text-align: center;   
    color: rgb(3, 3, 3);
    border-color: #f8c262;
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
}

.btnOkContainer button:hover{
    background-color: rgb(187, 187, 187);
}

.btnOkContainer button{
    /* padding-left: 12%; */
    width: 30vw;
    height: 6vh;
    text-align: center; 
    line-height: 6vh;
    color: rgb(36, 35, 35);
    margin: 0 auto;
    border: none;
    outline: none;
    display: flex;
    background-color: rgba(155, 155, 155, 0.336);
    font-size: 20px;
    border-radius: 4px;
    cursor: pointer;
}

.login{
    margin: 0 auto;
}
.note{
    display: flex;
    margin: 0 auto;
    margin-top: 4vh;
    color: #949494;
}

</style>
