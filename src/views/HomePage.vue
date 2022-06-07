<template>
 <!-- conditional if statement called v-if -->
<AddTask v-show="showAddTask" @add-task="addTask"/>

  <!-- if the data we are passing in as props is dynamic we need to v-bind it. It it sort of like binding methods to 'this' in react -->
  <!-- in MyTasks we are passing in a prop called tasks which is the tasks array -->
  <!--We are passing a prop @delete-task that is bound to this. into MyTasks which is the function deleteTask -->
  <MyTasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
    import MyTasks from '../components/MyTasks.vue'
    import AddTask from '../components/AddTask.vue'
    const URL = 'http://localhost:5000'
    export default {
        name: 'HomePage',
        props:{
            showAddTask: Boolean,
        },
        components: {
            MyTasks,
            AddTask,
        },
        data(){
            return{
                tasks:[]
            }
        },
        methods:{
    //everything at is getting data from the api we need to make asyc
    async addTask(task){
      //I am sending a task to the api  when i submit the form
      const res= await fetch(`${URL}/tasks`,{
        method: 'POST',
        headers:{
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })
      // i am getting back data which is the information i filled in the form in a json object form
      const data =await res.json()
      //when the form is submitted, we call this.tasks. We are still copying everything in tasks and adding a new task to the end
      //i am copying the current state of tasks and then adding to it the new task json object
      this.tasks =[...this.tasks, data]
    },

    async deleteTask(id){
      if(confirm('Are you sure?')){
        //making a delet request to the specific task so it will persist in the database
        const res= await fetch(`${URL}/tasks/${id}`,{
          method: 'DELETE',
        })

        res.status === 200 ? (this.tasks = this.tasks.filter((task)=>task.id !== id)) : alert('Error deleting task')
        //we are clicking on the icon in the MyTask component child and it's firing off this method so we have access to this.tasks
        //we are pasically setting state of this.tasks using filter. We are saying, any task.id that is not the id being passed in, we keep.
        //we are removing everything from this.tasks that is not the task with id being passed in
      }
    },
    //this is triggered when we click on the MyTask div, which emits to a toggle-reminder prop in MyTasks, which calls the toggleReminder function in App.vue
    async toggleReminder(id){
      //sets state of this.tasks: Maps through tasks and if the task.id is equal to the id being passed in (div being clicked) copy the task being currently mapped and set the reminder to the opposite of it's current value. else, just retain the current task.
     //this will find the task we want to toggle by making a get request and saves it in a variable
     const taskToToggle = await this.fetchTask(id)
     //copy the task to toggle in the database using spread and set the reminder to the opposite of it's current state
     const updTask ={...taskToToggle, reminder: !taskToToggle.reminder}
    //put request to update the database with the new updated task with toggled reminder state
     const res = await fetch(`${URL}/tasks/${id}`, {
       method: 'PUT',
       headers:{
         'Content-type': 'application/json'
       },
      //json takes only strings
       body: JSON.stringify(updTask)
     })
    //wait for the data to get back from the server and store it in a variable
     const data = await res.json()
    //update tasks by mapping through tasks and if the id is not the id being passed, copy the task object and set the reminder state to the new updated state, else retain the task 
     this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: data.reminder} : task)
    },
    //this function is fetching data from our json api on localhost 5000 'fake api' and returning the fetched data
    async fetchTasks(){
      const res = await fetch(`${URL}/tasks`)
      const data = await res.json()
      return data
    },
    //this function 
    async fetchTask(id){
      //this lets us find a single task from the API
      const res = await fetch(`${URL}/tasks/${id}`)
      const data = await res.json()
      return data
    }
  },

    //this is like...componentDid Mount 
    //when MyHeader is rendered, we are filling tasks:[] to be = to the array below. 
    //when a component is rendered, there are certain methods we can use to leverage that. sort of like componenetDidMount or useEffect
  // created(){
      //this is dummy data for now but we'll fill it with stuff from the api later
      //this is a bit like setting state of HeaderButton to be the array below.
    // this.tasks = [
    //     {
    //         id:1,
    //         text: 'Doctors Appointment',
    //         day: 'March 1st at 2:30pm',
    //         reminder: true,
    //     },
    //     {
    //         id:2,
    //         text: 'Meeting at School',
    //         day: 'March 3rd at 1:30pm',
    //         reminder: true,
    //     },
    //     {
    //         id:3,
    //         text: 'Food Shopping',
    //         day: 'March 3rd at 11:00am',
    //         reminder: false,
    //     },
    // ]   
// NOW OUR STATE or created() is asynchronous because we are fetching data for the state from the api and the array for tasks is just data from the fetchTasks method
  async created(){

    this.tasks = await this.fetchTasks()

  },
    }
</script>

<style lang="scss" scoped>

</style>