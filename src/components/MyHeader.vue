<template>
    <header>
        <h1>{{title}}</h1>
        <!-- we are passing text and colors as props into our component -->
        <!-- The component is reusable. Because we have passed in the props for what color and text we want, in the button we assign the value of color and text to be the props -->
        <!-- The text on the button is dynamic so we vbind it with : and set it to props passed in from App.vue -->
        <!-- conditional for the text. If the props showAddTask has a value of true (from App.vue state) then make the text Close else, make it Add Task-->
        <!-- because we've made the props emit more generic we can now reuse it later  -->
        <HeaderButton v-show="homePage" @btn-click="$emit('toggle-add-task')" :text="showAddTask ? 'Close' : 'Add Task'" :color="showAddTask? 'red' :'green'"/>
        <HeaderButton v-show="homePage"  text="Update Task" color="blue"/>
        <HeaderButton v-show="homePage" text="Delete Task" color="red"/>
    </header>
</template>

<script>
    import HeaderButton from './HeaderButton.vue'


    export default {    
        name: 'MyHeader',
        //declare my props in my component here
        props:{
            title:{
                type: String,
                default: 'Hello World', //we can have a default value for our prop and also specify the type of data
            },
            showAddTask: Boolean,
        },
        //WE MUST REGISTER ALL OUR COMPONENTS HERE.
        components:{
            HeaderButton
        },
        //we don't need to define the 'toggle-add-task' emit because no props are being passed up
        computed:{
            homePage(){
                //here we have acess to the route
                //if we're on the home page, then return true else return false
                //we are using this to show our add task button only on the home page
                if(this.$route.path === '/'){
                    return true
                }
                else{
                    return false
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    header{
        display:flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>