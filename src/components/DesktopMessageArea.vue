<template>
<div>
    <input name="typeZone" id="typezone" v-model="toSend">
    <button v-on:click="sendText" type="input">Send</button>
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
    #typeZone{
        width: 90%;
        height: 30%;
        resize: vertical;
    }
</style>