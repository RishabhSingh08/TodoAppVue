
<template>
    <div class="container">
        <h2 class="text-center mt-5">My Vue Todo App</h2>
    <!-- INPUT -->
        <div class="d-flex">    
            <input v-model="task" type="text" placeholder="Enter Text" class="form-control rounded-0">
            <button @click="submitTask" class="btn btn-warning rounded-0">
                <i class="fa fa-paper-plane"></i></button>
            <button @click="removeAll" class="btn btn-danger rounded-0"><i class="fa fa-skull"></i></button>
    </div>
    <!-- TABLE -->
        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                    <th scope="col">Task</th>
                    <th scope="col">Status</th>
                    <th scope="col"></th>
                    <th scope="col"></th>  
                </tr>
            </thead>
            <tbody v-for="(task, index) in tasks">
                <tr>
                    <td> 
                        <span :class="{'completed': task.status === 'Completed'}"> 
                            {{ task.name }} 
                        </span>
                    </td>
                    <td style="width: 110px;" @click="changeStatus(index)" class="pointer disable-selection" > 
                        <span
                            :class="{
                            'text-danger': task.status === 'To Do', 
                            'text-warning' : task.status === 'In Progress',
                            'text-success' : task.status === 'Completed'

                            }">
                            {{ task.status }} 
                        </span>
                    </td>
                    <td>
                        <div class="text-center" @click="editTask(index)">
                            <span class="fa fa-pen"></span>
                        </div>
                    </td>
                    <td>
                        <div class="text-center" @click="deleteTask(index)">
                            <span class="fa fa-trash"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>


</template>


<script>
export default{
    data() {
        return {
            task: 'Hello!',
            editedTask: null,
            statuses: ['To Do', 'In Progress', 'Completed'],
            tasks: [
                {
                    name: 'Steal Bananas from the Store',
                    status: 'To Do'
                },
                {
                    name: 'Walk the Fish',
                    status: 'In Progress'
                },
            ]
        }
    },
    methods: {
        submitTask(){
            if (this.task.length === 0) return

            if (this.editedTask === null) {
                this.tasks.push({
                    name: this.task,
                    status: 'To Do'
                })
            } else {
                this.tasks[this.editedTask].name = this.task
                this.editedTask = null
            }
            this.task = ''
        },

        deleteTask(index){
            this.tasks.splice(index, 1)
        },
        removeAll(){
            this.tasks = []
        },
        editTask(index){
            this.task = this.tasks[index].name
            this.editedTask = index
        },
        changeStatus(index){
            let newIndex = this.statuses.indexOf(this.tasks[index].status)
            if (++newIndex > 2) newIndex = 0
            this.tasks[index].status =  this.statuses[newIndex]
        }
    }

}

</script>


<style>
.pointer{
    cursor: pointer;
}

.completed{
    text-decoration: line-through;
}

.disable-selection{
    user-select: none;
}
</style>