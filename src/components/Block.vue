/* eslint-disable */
<template>
    <div class="block" :class="{play:padOn , pause:!padOn}">
            <img class="icon" :src='photo'>
    </div>
</template>

<script>

    export default {
        //data of a component is it's attrivutes (the data it owns)
        props: {
            name: String,
            file: Object,
            padOn: Boolean,
            photo: String,
        }, 
        data() {
            return {
               
            }
        },
    }
</script>

<style scoped>

.block {
    margin: 10px;
    border-radius: 10px;
    padding: 50px;
    color: white;
}
.block:active{
    transform: scale(0.85);
}

.block:hover{
    opacity: 0.8;
    cursor: pointer;
}
.play{
    background-color: rgb(111, 226, 111)
}
.pause{
    background-color: rgb(226, 111, 111)
}
.icon{
    height: 50px;
    width: 50px;
}


</style>