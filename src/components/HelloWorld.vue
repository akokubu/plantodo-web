<template>
  <div class="hello">
    new Task: <input type="text" v-model="newTask" @keyup.enter="addTask">
    <ul>
      <li v-for="task in tasks">
        <span v-if="task.editMode" 
          @keyup.enter="saveTask(task)"
          @keyup.esc="cancelEdit(task)"><input type="text" v-model="task.name"> </span>
        <div v-else>
          <input type="checkbox" v-model="task.done">
          <span @click="editTask(task)">{{ task.name }} </span>
          <span @click="deleteTask(task)">[x]</span>
        </div>
      </li>
    </ul>
    <button @click="deleteAll">Delete done</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      tasks: [
        { name: 'todo 1', done: false, editMode: false },
        { name: 'todo 2', done: false, editMode: false },
        { name: 'todo 3', done: true, editMode: false },
      ],
      newTask: "",
    }
  },
  methods: {
    addTask: function(event) {
      //let text = this.newTask && this.newTask.trim()
      let text = this.newTask.trim()
      if (!text) {
        return
      }
      this.tasks.push({
        name: this.newTask,
        done: false,
        editMode: false
      })
      this.newTask = ""
    },
    deleteTask: function(task) {
      this.tasks.splice(this.tasks.indexOf(task), 1)
    },
    deleteAll: function(event) {
      for (let i=this.tasks.length - 1; i>=0; i--) {
        if (this.tasks[i].done) {
          this.tasks.splice(i, 1)
        }
      }
    },
    editTask: function(task) {
      task.editMode = true
      task.cache = task.name
    },
    saveTask: function(task) {
      task.editMode = false
    },
    cancelEdit: function(task) {
      task.name = task.cache
      task.editMode = false
      console.log(task.cache)
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
