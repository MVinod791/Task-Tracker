<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label for="addTask">Task</label>
            <input type="text" v-model="text" name="text" placeholder="Add Task" id="addTask">
        </div> 
        
        <div class="form-control">
            <label for="day">Day & Time</label>
            <input type="text" placeholder="Add Day and Time" v-model="day" name="day" id="day">
        </div> 
        <div class="form-control form-control-check">
            <label for="checkbox">Show Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder" id="checkbox">
        </div> 
        <input type="submit" value="Save Task" class="btn btn-block" >
    </form>
</template>

<script>
export default {
    name:'AddTask',
   
    data(){
        return{
            text:'',
            day:'',
            reminder:false,
        }
    },

    methods: {
        onSubmit(e){
            e.preventDefault();
            if(!this.text){
                alert("Please add task...");
                return
            }

            const newTask={
                //id:Math.floor(Math.random()*1000),
                text:this.text,
                day:this.day,
                reminder:this.reminder
            }

            //console.log(newTask);
            //passing to higher level where we need to add data present
            this.$emit('add-task',newTask)

            this.text='',
            this.day='',
            this.reminder=false
        }
    },
}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}

</style>