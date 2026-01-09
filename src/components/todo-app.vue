<template>
<div class="container" style=" max-width:600px;">
    <h2 class="text-center mt-5">My Todo App</h2>

    <div class="d-flex mt-5">
        <input type="text" class="w-100 form-control " placeholder="Enter Task" v-model="task">

        <button class="btn btn-warning" type="submit" @click="submitTask">submit</button>
    </div>

    <div class="mt-5 border rounded p-3">
        <table class="table">
            <thead>
                <tr class="text-center">
                    <th scope="col">Task</th>
                    <th scope="col" style="width:120px;">Status</th>
                    <th scope="col">Edit</th>
                    <th scope="col">Delete</th>
                </tr>
            </thead>

            <tbody>

                <tr v-for="(item,index) in tasks" :key="index" class="text-center">
                    <td :class="{'line-through': item.status==='finished'}">{{ item.name }}</td>

                    <td style="cursor:pointer;" :class="{
                        'text-danger': item.status==='to-do',
                        'text-warning': item.status==='in-progress',
                        'text-success': item.status==='finished'
                    }" @click="changeStatus(index)">
                        {{ capitalizeFirstChar(item.status) }}
                    </td>
                    <td>
                        <span @click="editTask(index)">
                            <i class="fa fa-edit text-primary" style="cursor:pointer;"></i>
                        </span>
                    </td>
                    <td>
                        <span @click="deleteTask(index)">
                            <i class="fa fa-trash text-danger" style="cursor:pointer;"></i>
                        </span>
                    </td>
                </tr>
            </tbody>

        </table>
    </div>
</div>
</template>

<script>
export default {
    name: 'TodoApp',
    props: {
        msg: String
    },
    data() {
        return {
            task: "",

            editedTask: null,

            statuses: ["to-do", "in-progress", "finished"],
            tasks: [{
                    name: "Review project requirements",
                    status: "to-do"
                },
                {
                    name: "Develop core application features",
                    status: "in-progress"
                },
                {
                    name: "Perform final testing and deployment",
                    status: "finished"
                }
            ]

        }
    },
    methods: {
        capitalizeFirstChar(str) {
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        changeStatus(index) {
            let newIndex = this.statuses.indexOf(this.tasks[index].status);
            if (++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.statuses[newIndex];
        },

        editTask(index) {
            this.task = this.tasks[index].name;
            this.editedTask = index;
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },

        submitTask() {
            if (this.task.trim().length === 0) return;

            if (this.editedTask !== null) {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            } else {
                this.tasks.push({
                    name: this.task,
                    status: "to-do"
                });
            }

            this.task = "";
        }

    },
};
</script>

<style scoped>
.line-through {
    text-decoration: line-through;
    color: gray;
}

.text-denger {
    color: red;
}

.text-waening {
    color: orange;
}
</style>
