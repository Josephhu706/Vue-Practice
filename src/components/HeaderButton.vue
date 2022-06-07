<template>
    <div>
        <!-- This component is just a button that we're importing into our MyHeader component -->
        <!-- remember we are passing in our color as props from the parent Header -->
        <!-- @click is the same as saying v.bind:click. We are passing in a click event handler when we click te button-->
        <button @click="onClick()" :style="{background: color}" class="btn">{{text}}</button>
    </div>
</template>

<script>
    export default {
        name: 'HeaderButton',
        //when we pass in props from parent to child we need to define our props
        props:{
            //define our data type after defining or props passed in
            text: String,
            color: String
        },
        // define methods because we are passing in an onClick method into the button. 
        methods:{
            //to make this button reusable, we change the emit from 'toggle-add-task' to btn-click
            onClick(){
                //this emits toggle-add-task to the parent which is MyHeader. We are not emiting any specific prop, it's only job is toggle the form.
                //we don't need to pass any props up to the parent, just to trigger a function in Appp.vue through MyHeader
                // this.$emit('toggle-add-task')
                this.$emit('btn-click')
            },
        },

    }
</script>

<style lang="scss" scoped>

</style>