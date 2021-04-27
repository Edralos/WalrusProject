<template>
  <div id="tapZone"></div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    friendId: String,
    socket: Object,
    reverse: Object
  },
  data() {
    return {
      ongoingTouch: Object,
      touchCount: 0,
      datestart: 0,
      dateend: 0,
      letter: "",
      message: "",
    };
  },
  methods: {
    touchStartHandler(ev) {
      ev.preventDefault();
      console.log("tapstart");
      let touches = ev.changedTouches;
      this.ongoingTouch = touches[0];
      this.datestart = Date.now();
      if (this.datestart - this.dateend > 400) {
        console.log("space");
        console.log("letter : " + this.letter);
        if(this.reverse[this.letter] != undefined){
          
          this.message += this.letter + " ";
        }
          
        

        
        this.letter = "";
      }
      
    },
    touchEndHandler(ev) {
      ev.preventDefault()
      console.log("tapend");
      let touches = ev.changedTouches
      
      if(this.ongoingTouch.identifier != touches[0].identifier){
          return
      }
      let end = Date.now();
      let elapsed = end - this.datestart;
      console.log("end elapsed : " + elapsed);
      
      if (elapsed < 100) {
        console.log(".");
        this.letter += ".";
      }
      else if (elapsed > 1000) {
        console.log("sending");
        this.message += this.letter + " ";
        this.letter = "";
        try {
          let packet = {
            id: this.friendId,
            morse: this.message
          }
          this.socket.emit("messageMorse", packet);
          console.log("message : " + packet.morse + " to : " + packet.id);
          
        } catch (error) {
          console.log(error);
          
        }
        this.message = "";
      } else {
        console.log("-");
        this.letter += "-";
      }
      this.dateend = Date.now();
      return;
    }


  },
  mounted() {
    let taptap = document.getElementById("tapZone");
    if(taptap == null)
    {
      console.log("taptap absent");
      
      return
    }
    console.log("taptap présent");
    
    taptap.addEventListener("touchstart", this.touchStartHandler, false);
    taptap.addEventListener("touchend", this.touchEndHandler, false);

    
  },
});
</script>


<style scoped>
#tapZone {
  height: 90px;
  border-style: dashed;
  border-color: #c6f8ff;
}
@media (min-width: 415px) {
  #tapZone {
    display: none;
  }
}
</style>