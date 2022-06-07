<template>
<!-- v-for is a for loop. In this case it's a for in loop -->
    <div  :key="task.id" v-for="task in tasks" >
        <!-- here we are passing props into MyTask which is the empty state array of tasks from MyHeader,populated by App.vue using created() -->
        <!--Then the tasks array is passed into MyTasks, looped through, whith each task object being passed as props down to MyTask which is bound because it is dynamic-->
        <!-- @delete-task is a prop passed down from App.vue. We are passing that down even further down to MyTask as an event-handler $emit  -->
        <MyTask @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task"/>
    </div>
</template>

<script>
import MyTask from './MyTask.vue'
    export default {
        name: 'MyTasks',
        //defining the parameters fro the props.
        //this is saying. the only props we can pass into this component are the ones below, in the data format specified
        //we are passing in tasks as a prop from App.vue which is an array.
        //so MyHeader has a state called tasks which is []. We are importing MyHeader to App.vue, the parent.
        //we are setting the state of MyHeader tasks[] with dummy info when the component mounts using created() and then passing that as props to MyTasks.
        //My tasks is taking the tasks[] state as props and iterating it into h3 tags.
        props:{
            tasks: Array
        },
        components:{
            MyTask,
        },
        //emits are passed upwards. We need to define emits passed up. props are passed down, emits are passed up
        emits: ['delete-task', 'toggle-reminder'],

        
    }
</script>

<style lang="scss" scoped>

</style>