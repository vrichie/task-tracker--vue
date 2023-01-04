<template>
    <div :key="task.id" v-for="task in tasks">
        <!-- <h3>{{task.text}}</h3> -->
        <RealTask @toggle-reminder="$emit('toggle-reminder',task.id)" @delete-task="$emit('delete-task',task.id)" :task="task"/>


    </div>
    
</template>

<script>
import RealTask from './RealTask';
export default {
    name:"TaskFile",
    props:{
        tasks:Array
    },
    components:{
        RealTask
    },
    emits:['delete-task',"toggle-reminder"],
}
</script>

<style scoped>

</style>