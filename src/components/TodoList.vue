<template>
  <br>
  <div class="container-fluid">
    <div class="row">
      <div class="col-10">
        <h3>My ToDo List</h3>
        <br/>
        <div class="row mx-0">
          <div class="col-10 p-0">
            <input class="form-control" type="text" v-model="task"  @keyup.enter="addTask" style="border: 1px solid green !important;"/>
          </div>
          <div class="col-2 p-0">
            <button class="btn btn-success w-100" type="button" @click="addTask" :class="{'disabled': !task}">
              Add Task
            </button>
          </div>
        </div>
        <div v-if="todoList.length > 0">
          <br/>
          <h4>Todo</h4>
          <div @drop="dropItem($event)">
            <ul class="list-group">
              <li v-for="t in todoList" 
              :key="t.taskNo" 
              :id="t.taskNo"
              class="list-group-item p-0" 
              draggable="true" 
              @dragstart="dragItem($event, t.taskNo)"
              @dragenter.prevent
              @dragover.prevent>
                <div class="row m-0">
                    <div class="col-1 p-0">
                      <i class="bi bi-square btn w-100" @click="markCompleted(t.taskNo)" style="color:green;"></i>
                    </div>
                    <div class="col-10 p-0">
                      <p class="m-0 p-1">{{ t.task }}</p>
                    </div>
                    <div class="col-1 p-0">
                      <i class="bi bi-x-square-fill btn w-100" @click="removeTask(t.taskNo)" style="color:red;"></i>
                    </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
        <div v-if="completedList.length > 0">
          <br/>
          <h4>Completed</h4>
          <ul class="list-group" >
            <li v-for="tc in completedList" :key="tc" class="list-group-item py-0">
              <div class="row m-0">
                <div class="col-1 p-0">
                  <i class="bi bi-check-square-fill btn w-100" style="color:green;cursor:auto;"></i>
                </div>
                <div class="col-11 p-0">
                  <p class="m-0 p-1"><s>{{ tc.task }}</s></p>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue'

export default {
  name: 'TodoList',
  setup(){
    const task = ref('')
    const todoList = ref([])
    const completedList = ref([])

    const addTask = () => {
      task.value = task.value.trim()
      if( task.value){
        todoList.value.push({
          'task': task.value,
          'taskNo': todoList.value.length+1
        })
        task.value = ""
      }
    }

    const removeTask = (t_no) => {
      todoList.value = todoList.value.filter( t => 
        t.taskNo !== t_no)
    }

    const markCompleted = (t_no) => {
      let itemVal = todoList.value.filter( t => 
          t.taskNo === t_no
      )
      if(itemVal.length === 1){
        completedList.value.push(itemVal[0])
      }
      removeTask(t_no)
    }

    const dragItem = (event, taskNo) => {
      event.dataTransfer.dropEffect = 'move'
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setData('taskNo', taskNo)
    }

    const dropItem = (event) => {
      var taskNo = event.dataTransfer.getData('taskNo');
      let dragElement = document.getElementById(`${taskNo}`)
      if(dragElement){
        let parentElement = dragElement.parentElement
        let targetElement = event.target.offsetParent
        if(dragElement !== targetElement){
          parentElement.removeChild(dragElement)
          targetElement.insertAdjacentElement('afterend',dragElement)
        }
      }
    }

    return {
      task, todoList, completedList, addTask, removeTask, markCompleted, dropItem, dragItem
    }
  }
}
</script>

<style scoped>
input:focus, button:focus{
  outline: none !important;
}
li:nth-child(even){
  background: #f2f2f2 !important;
}
</style>
