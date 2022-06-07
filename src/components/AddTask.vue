<template>
<!-- call a method @submit="" that will be emited to App.vue -->
   <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <!-- v-model lets us set the data(){} 'state' so the text: can take the input for this input. this is like binding inpu to the data -->
      <!-- whaetver is in this input will set the data and whatever the data is set to so will the input -->
      <!-- v-model is 2 way binding utilising an onChange  -->
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
    export default {
        name: 'AddTask',
        data(){
            return{
                text:'',
                day:'',
                reminder: false,
            }
        },
        methods:{
            onSubmit(e){
                //like preventDefault in react. prevents form from redirecting us or refreshing the page when submitted
                e.preventDefault()
                //if there is no text in the input when submitted,alert the user
                if(!this.text){
                    alert('Please add a task')
                    return
                }

                const newTask = {
                    // id:Math.floor(Math.random()*100000),
                    //whatever is in the form input text
                    text: this.text,
                    day: this.day,
                    reminder: this.reminder
                }
                //We now need to emit this newTask upwards to the parent so we can display it in the tasks.
                this.$emit('add-task', newTask)
                // here we are clearing the form after a newTask has been created
                this.text= ''
                this.day=''
                this.reminder=false
                
                console.log(newTask)

                

            }
        }
    }
</script>

<style lang="scss" scoped>
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