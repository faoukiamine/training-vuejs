<template>
    <div class="container col-6">
        <h2 class="text-center mt-5">To-Do App</h2>
        
        <!-- Input -->
        <div class="d-flex mt-5">
            <input v-model="task" type="text" class="form-control" placeholder="Enter task">
            <button @click="submitTask" class="btn btn-warning">Submit</button>
        </div>

        <!-- Task table -->
        <table class="table table-bordered mt-3">
            <thead>
                <tr>
                    <th class="col">Task</th>
                    <th class="col">Status</th>
                    <th class="col text-center">Edit</th>
                    <th class="col text-center">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td>
                        <span :class="{'finished': task.status === 'Finished'}">
                            {{ task.name }}
                        </span>
                    </td>
                    <td style="width: 120px">
                        <span @click="changeStatus(index)" class="pointer"
                            :class="{
                                'text-danger' : task.status === 'To-do',
                                'text-warning' : task.status === 'In-progress',
                                'text-success' : task.status === 'Finished'
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
export default {
    name: "ToDoApp",
    
    data(){
        return {
            task: '',
            editedTask: null,
            availableStatus: ['To-do', 'In-progress', 'Finished'],
            tasks: []
        }
    },

    methods: {
        submitTask(){
            if(this.task.length === 0) return;

            if(this.editedTask === null){
                this.tasks.push({
                    name: this.task,
                    status: this.availableStatus[0]
                });
            }
            else{
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            }


            this.task = '';
        },

        deleteTask(index){
            this.tasks.splice(index, 1);
        },

        editTask(index){
            this.task = this.tasks[index].name;
            this.editedTask = index;
        },

        changeStatus(index){
            let currentIndex = this.availableStatus.indexOf(this.tasks[index].status);
            if(++currentIndex > 2) currentIndex = 0;
            this.tasks[index].status = this.availableStatus[currentIndex];
        }
    }
}
</script>

<style scoped>
    .pointer {
        cursor: pointer;
    }
    .finished {
        text-decoration: line-through;
    }
</style>