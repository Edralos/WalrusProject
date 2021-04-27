<template>
<div>
    <input name="typeZone" id="typezone" v-model="toSend">
    <button v-on:click="sendText" type="input"><img src="../assets/img/sendMsg.webp" alt="Send">
        </button>
</div>
</template>

<script>
export default{
    name: 'DesktopMessageArea',
    data(){
        return{toSend: ''}
    },
    props:{
        friendId: String,
        alphabet: Object,
        socket : Object,
        },
    methods:{
        sendText(){
            let lower = this.toSend.toLowerCase();
            let mrs = "";
            for (let index = 0; index < lower.length; index++) {
                mrs+= this.alphabet[lower[index]] + " ";
            }
            let packet = {
                 id:this.friendId,
                 morse: mrs
                 }
            this.socket.emit('messageMorse', packet)
        }
    }

}
</script>

<style scoped>
    input{
        width: 100%;
        max-width: 1000px;
        height: 100%;
        resize: vertical;
    }
    div{
        display: flex;
        justify-content: center;
        height: 70px;
    }

    button{
        height: 100%;
        background: transparent;
        border-color: transparent;
        
    }

    
    @media (max-width: 414px) {
        div{
            display: none;
        }
    }
</style>