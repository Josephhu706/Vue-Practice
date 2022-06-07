<template>
<!-- we bind class because it is dynamic. If class.reminder is true, set class to reminder, else nothing. we also add as second class of 'task' to make the color green -->
    <!-- pass in double click method that emits straight in the div. We don't have to define a method handler for it -->
    <!-- we need to emit toggle-reminder to the parent, App.vue and pass in task.id -->
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
        <!-- takes the text from the task object prop being pased int My Task and renders it on the page -->
        <!-- event handler delete function passed into icon. when onDelete is triggered with a click, it triggers this.$emit passed from MyTasks as props, passing in the task.id -->
        <h3>{{task.text}}<i @click="$emit('delete-task', task.id)" class="fas fa-times"></i></h3>
        <!-- font awesome icon from cdn -->
        <p>{{task.day}}</p>
    </div>
</template>

<script>
    export default {
        name: 'MyTask',
        //receives task which is being looped through in MyTasks as a prop which is a data type Object
        props:{
            task: Object
        },
        //function for event handler for icon
        methods:{
            // onDelete(id){
            //     // since this task component is 2 levels deep: App.vue --> MyTasks.vue --> MyTask, we need to pass it up to the parent 2 levels.
            //     //using #emit lets us do this. we pass in the id as a the parameter we are using delete-task on.
            //     //we are using$emit to trigger the event handler 'delete-task' for the specific task.id being passed in
            //     this.$emit('delete-task', id)
            // },
        },
        emits:["toggle-reminder"]
    }
</script>

<style lang="scss" scoped>
.fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

</style>