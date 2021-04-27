<template>
<div id="mainDiv">
  <div id="titleZone">
      <img class="walter" src="./assets/img/smWalrus.webp" alt="Premier Walrus" >
      <header>The Walrus Project</header>
      <img class="walter" id="wally" src="./assets/img/smWalrus.webp" alt="Second walrus" >
  </div>
    <p>Your id : {{idUser}}</p>
    <input type="text" id="friendIdField" placeholder="Write Target ID here" v-model="friendId"/>
  <body>
    <description/>
    <chat-history :lastMessage="lastMessage"/>
    <desktop-message-area :alphabet="alphabet" :friendId="friendId" :socket="socket"/>
    <phone-message-area :friendId="friendId" :socket="socket" :reverse="reverseAlph"/>
  </body>
  <footer>A project by Ulysse HAVE & Thibaud MORNET BLANCHET</footer>
</div>
  
</template>

<script>
import ChatHistory from './components/ChatHistory.vue'
import Description from './components/Description.vue'
import morse from './assets/json/morse.json'
import reverseWalrus from './assets/json/reverseWalrus.json'
import DesktopMessageArea from './components/DesktopMessageArea.vue'
import PhoneMessageArea from './components/PhoneMessageArea.vue'

export default {
  name: 'App',
  components: {
    ChatHistory,
    Description,
    DesktopMessageArea,
    PhoneMessageArea
  },
  data(){
    return {
      idUser: '',
      friendId: '',
      socket:'',
      alphabet : morse,
      reverseAlph : reverseWalrus,
      lastMessage: 'Here will appear your last received message'
      };
  },
  created(){
    console.log(this.reverseAlph)
    let vm = this;
    console.log('coucou: ' + window.location.href);
    let io = require("socket.io-client");

    this.socket = io.connect(window.location.href.replace('8080','5000'), {
      reconnectionDelayMax: 10000,
      
    });

    this.socket.on('connect', function() {
      console.log('Connected to server');
      vm.socket.emit('message', 'CA MARCHE');
    });

    this.socket.on('idResponse',function(id){
      vm.idUser = id;
      console.log('id attribué ' + id)
    });

    this.socket.on('messageMorse', function(morse){
        console.log("received smtg : " + morse);
        let arr =[];
        let strW = morse.split(' ');
        console.log("strW :" + strW);
        let strH = "";
        strW.forEach(letter => {
          if(letter != '')
          {strH += vm.reverseAlph[letter];}
        });
        console.log(strH);
        vm.lastMessage = strH.toString().toUpperCase();

        for (let index = 0; index < morse.length; index++) {
            switch (morse[index]) {
              case '.':
                arr.push(150);
                arr.push(40);
                break;
              case '-':
                arr.push(300);
                arr.push(40);
                break;
              case ' ':
                arr.push(1);
                arr.push(600);
                
                break;
              default:
                break;
            }
        }
       window.navigator.vibrate(arr);
    });
  }
}
</script>

<style>
  #mainDiv{
    height: 100%;
  }
  #titleZone{
    display: flex;
    justify-content: center;
  }

  #wally{
    transform: scaleX(-1);
  }

  .walter{
    max-width: 10%;
    min-width: 50px;
    max-height: 90px;
    min-height: 50px;
    align-self: center;
  }

  #friendIdField{
    width: 90%;
    color: #c6f8ff;
    background-color: #282a31;
    border-color: #c6f8ff;
    height: 20px;
    resize: unset;
    max-width: 300px;
  }


  @media (min-width: 415px) {
    header{
      font-size: 120%;
    }
  

  }
</style>
