<template>
  <div id="tapZone"></div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    friendId: String,
    socket: Object,
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
  created() {
    let taptap = document.getElementById("tapZone");
    taptap.addEventListener("touchstart", touchStartHandler, false);
    taptap.addEventListener("touchend", touchEndHandler, false);

    function touchStartHandler(ev) {
      ev.preventDefault();
      let touches = ev.changedTouches;
      this.ongoingTouch = touches[0];
      this.datestart = Date.now();
      if (this.datestart - this.dateend > 600) {
        this.message += this.letter + " ";
        this.letter = "";
      }
    }

    function touchEndHandler(ev) {
      ev.preventDefault()
      let touches = ev.changedTouches
      if(this.ongoingTouch != touches[0]){
          return
      }
      let end = Date.now();
      let elapsed = end - this.datestart;
      if (elapsed < 30) {
        this.letter += ".";
      }
      if (elapsed > 1000) {
        this.message += this.letter + " ";
        this.letter = "";
        this.socket.emit("messageMorse", this.message);
        this.message = "";
      } else {
        this.letter += "-";
      }
      this.dateend = Date.now();
      return;
    }
  },
});
</script>


<style scoped>
div {
  height: 90px;
  border-style: dashed;
  border-color: #c6f8ff;
}
@media (min-size: 380px) {
  div {
    display: none;
  }
}
</style>