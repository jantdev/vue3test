<template>
    <header>
        <h1>{{ title }}</h1>
        <Button class="btn" v-show="homepage" :title="showAddTask ? 'Close' : 'Add Task'" :color="showAddTask ? 'red' : 'green'"  @click="$emit('toogle-form')"/>
    </header>
</template>


<script>
import Button from "./Button";

export default {
    name:'Header',
    props:{
        title:{
            type:String,
            default:'hello world'
        },
        showAddTask:Boolean
    },
    components:{Button},
    emits:['toogle-form'],
    computed:{
        homepage(){
            if(this.$route.path === '/' ){
                return true
            }else{
                return false
            }
        }
    }
}
</script>

<style lang="css" scoped>
header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    }
</style>