<template>
    <div>
        <p>{{lastMessage}}</p>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
    props:{
        lastMessage:String
    },
    data(){
        return {
        }
    },
    created(){
        console.log(this.lastMessage);
        
    }
})
</script>


<style scoped>

</style>