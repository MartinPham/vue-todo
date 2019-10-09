<template>
  <div>
    <input v-model="newTask"/>
    <button v-on:click="addTask">Add</button>
    <hr/>
    <ul>
      <li v-for="task in tasks" v-bind:key="task.id">
        <div v-if="editingTasks.indexOf(task) === -1">
          {{task.name}} 
          <button v-on:click="editTask(task.id)">Edit</button>
          <button v-on:click="removeTask(task.id)">Delete</button>
        </div>
        <div v-if="editingTasks.indexOf(task) > -1">
          <input v-model="editingTasks[editingTasks.indexOf(task)].name"/>
          <button v-on:click="saveTask(task.id)">OK</button>
          <button v-on:click="cancelEditingTask(task.id)">Cancel</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import uuidv4 from 'uuid/v4'


export default {
  name: 'app',
  data: () => ({
    newTask: '',
    editingTasks: [],
    tasks: [
      {
        id: uuidv4(),
        name: 't1'
      },
      {
        id: uuidv4(),
        name: 't2'
      },
    ]
  }),
  methods: {
    addTask() {
      this.tasks.push({
        id: uuidv4(),
        name: this.newTask
      })
      this.newTask = ''
    },
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    },
    editTask(id) {
      for(let task of this.tasks)
      {
        if(task.id === id)
        {
          this.editingTasks.push(task)
          break
        }
      }
    },
    saveTask(id) {
      for(let etask of this.editingTasks)
      {
        if(etask.id === id)
        {
          for(let task of this.tasks)
          {
            if(task.id === id)
            {
              task = etask
              break
            }
          }
          break
        }
      }
      this.cancelEditingTask(id)
    },
    cancelEditingTask(id) {
      this.editingTasks = this.editingTasks.filter(task => task.id !== id)
    }
  }
}
</script>

<style>

</style>
