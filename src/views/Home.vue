<template>
  
  <AddTask v-if="showAddTask" @add-task="addTask"/>

  <Tasks @delete-task="deleteTask" @toogle-reminder="toogleReminder" :tasks="tasks"/>
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
    name:"Home",
    components:{
        Tasks,
        AddTask,
    },
    props:{
        showAddTask:Boolean
    },
    data(){
        return{tasks:[]}
        
    },
    methods:{
         async deleteTask(id){
     const res = await fetch(`api/tasks/${id}`,{
       method:'DELETE'
     })

     res.status === 200 ? (this.tasks = this.tasks.filter((task)=>{
        return task.id !== id;
      })) : console.log(res.status);
      
    },
    async toogleReminder(id){

      const taskToToogle = await this.fetchTask(id)

      const updateReminder = {...taskToToogle,reminder: !taskToToogle.reminder}

      const res = await fetch(`api/tasks/${id}`,{
        method:'PUT',
         headers:{
          'Content-type':'application/json'
        },
          body:JSON.stringify(updateReminder)
      })

      const data = await res.json();
       this.tasks= this.tasks.map((task)=> 
        task.id === id ? {...task, reminder: !data.reminder} : task)
    },
    async addTask(task){
      const res = await fetch('api/tasks',{
        method:'POST',
        headers:{
          'Content-type':'application/json'
        },
        body:JSON.stringify(task)
      })
      const data = await res.json();
        this.tasks = [...this.tasks,data]
    },
      async fetchData(){
        const res= await fetch('api/tasks')
        const data = await res.json();
        return data
      },
      async fetchTask(id){
        const res= await fetch(`api/tasks/${id}`)
        const data = await res.json();
        return data
      }
    },
     async created(){

  
    this.tasks = await this.fetchData();

  } 
}

</script>