<template>
    <AddTask v-show="showAddTask" @add-task="addTask"/>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'
export default {
    name:'Home',
    props:{
        showAddTask:Boolean
    },
    components:{
        Tasks,
        AddTask,
    },
    data(){
    return{
      tasks:[],
    }
  },
  methods:{

    async addTask(newTask){
      const res=await fetch('api/tasks',{
        method:'POST',
        headers:{
          'Content-type':'application/json'
        },
        body:JSON.stringify(newTask)
      })

      const data=await res.json()
      
      this.tasks=[...this.tasks,data];
    },
    async deleteTask(id){
     //console.log('task',id)
     if(confirm("Are you sure?")){
       const res=await fetch(`api/tasks/${id}`,{
         method:'DELETE',
       })
        res.ok===true ? 
        (this.tasks=this.tasks.filter((task)=>task.id!==id))
        : alert("Error While deleting")
     }
    },
    async toggleReminder(id){
      //console.log('task',id);
      const toggleTask=await this.fetchTask(id)
      const updTask={...toggleTask, reminder:!toggleTask.reminder}
      const res=await fetch(`api/tasks/${id}`,{
        method:'PUT',
        headers:{
          'Content-type':'application/json',
        },
        body:JSON.stringify(updTask)
      })

      const data=await res.json()

      this.tasks=this.tasks.map((task)=>
      task.id===id ? {...task,reminder:data.reminder}:task
      
      )
    },
    async fetchTasks(){ 
      const res= await fetch('http://localhost:5001/tasks')
      //console.log(response)

      const data= await res.json()

      return data
    },
    async fetchTask(id){ 
      const res= await fetch(`http://localhost:5001/tasks/${id}`)
      //console.log(response)

      const data= await res.json()

      return data
    }
  },
  async created(){
    this.tasks= await this.fetchTasks()
  }

}
</script>