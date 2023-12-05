<template>
    <div class="flex align-middle justify-center flex-col w-full">
        <div class="flex justify-center align-middle flex-col items-center">
            <h1 class="text-2xl font-bold">Todo List</h1>
            <div class="flex flex-row w-full">
                <input v-model="inputValue" class="border w-full h-10 text-xl p-2 rounded-md" type="text" />
                <button v-on:click="addNewTask" class="w-40 hover:bg-gray-400 transition-all hover:text-white rounded-md">Add Task</button>
            </div>
        </div>
        <div v-if="tasksArray.length == 0" class="mt-10">
            <p class="text-2xl">No new Tasks</p>
        </div>
        <div v-if="tasksArray.length !== 0" class="w-3/4 mx-auto my-0 mt-5">
            <div class="flex flex-row justify-between" v-for="(item, index) in tasksArray" :key="item.id">
                <input v-on:click="item.isDone = !item.isDone" class="mr-5" type="checkbox" />
                <span :style="{ 'text-decoration': item.isDone ? 'line-through' : 'none' }" class="w-10">
                    {{ index +1 }} .-
                </span>
                <div class="w-full flex flex-row justify-between" v-if="!item.isEditing">
                    <span :style="{ 'text-decoration': item.isDone ? 'line-through' : 'none' }">{{ item.title }}</span>
                    <div>
                        <button :disabled="item.isDone" v-on:click="isModifyingFunc(item)" class="w-20 hover:bg-gray-400 transition-all hover:text-white rounded-md">Edit</button>
                        <button v-on:click="deleteTask(index)" class="w-20 hover:bg-gray-400 transition-all hover:text-white rounded-md">Delete</button>
                    </div>
                </div>
                <div class="flex flex-row w-full" v-if="item.isEditing">
                    <input v-model="modInputValue" class="border w-full h-7 text-md p-2 rounded-md" type="text" />
                    <button 
                        v-on:click="modTask(item)" 
                        class="w-40 hover:bg-gray-400 transition-all hover:text-white rounded-md">
                        Mod Task
                    </button>
                    <button
                        v-on:click="isModifyingFunc(item)"
                        class="w-40 hover:bg-gray-400 transition-all hover:text-white rounded-md">
                        Cancel
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue'

    // var count = 1
    const inputValue = ref('')
    const modInputValue = ref('')
    const tasksArray = ref([])
    const isModifying = ref(null)
    // const modTask = ref(null)

    function addNewTask() {
        if (!inputValue.value){
            console.log(inputValue)
        } else{
            const task = {
                id: crypto.randomUUID(),
                // count: count,
                isDone: false,
                isEditing: false,
                title: inputValue.value
            }
            // count += 1
            tasksArray.value.push(task)
            inputValue.value = ''
        }
    }

    function isModifyingFunc(task) {
        task.isEditing = !task.isEditing
    }

    function modTask(task){
        task.title = modInputValue.value
        task.isEditing = !task.isEditing
    }
    
    function deleteTask(index){
        tasksArray.value.splice(index, 1)
    }
</script>