<template>
    <button @click="this.$emit('toogle-form')" :style="{background:color}">{{title}}</button>
</template>


<script>

export default{
    
    name:'Button',
    props:{
        title:String,
        color:String,
    },
  
    }
</script>


<style>



</style>