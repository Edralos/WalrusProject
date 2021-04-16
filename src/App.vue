<template>
<div>
  <div id="titleZone">
      <img class="walter" src="src/assets/img/smWalrus.webp" alt="Premier Walrus" >
      <header>The Walrus Project</header>
      <img class="walter" src="src/assets/img/smWalrus2.webp" alt="Second walrus" >
  </div>
  <body>
      <description/>
      <chat-history/>

  </body>
  <footer>A project by Ulysse HAVE & Thibaud MORNET BLANCHET</footer>
</div>
  
</template>

<script>
import ChatHistory from './components/ChatHistory.vue'
import Description from './components/Description.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
  #titleZone{
    display: flex;
    justify-content: center;
  }

  .walter{
    max-width: 10%;
    min-width: 50px;
    max-height: 90px;
    min-height: 50px;
    align-self: center;
  }

  @media (min-width: 380px) {
    header{
      font-size: 120%;
    }
  }
</style>
