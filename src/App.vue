<template>
  <div class="container">
    <div id="taskApp" class="row justify-content-md-center">
      <div class="col-sm-4">
        <h1 style="color: #fff;">{{ nameApp }}</h1>
        <form v-on:submit="addTask">
          <input type="text" v-model="newTask.title" class="form-control" ref="newTask" style="width: 70%">
          <button type="submit" class="btn btn-success ml-1">Add Task</button>
        </form>
        <div 
          v-if="showMessage" 
          v-bind:class="typeMessage == 'success' ? 
                          'alert alert-success' 
                        : 
                         typeMessage == 'warning' ?
                          'alert alert-warning'
                         :
                          'alert alert-danger'" 
          role="alert"
        >
          {{  message }}
        </div>
      </div>
      <div class="col-sm-3 table-responsive-lg">
        <table v-if="tasks.length > 0" class="table table-dark table-striped">
          <thead >
            <tr>
              <th scope="col">Done</th>
              <th scope="col">Task</th>
              <th scope="col">Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="task in tasks" v-bind:class="task.done? 'bg-info' : ''" :key="task.title" >
              <td >
                <input type="checkbox" v-model="task.done">
              </td>
              <td>
                <!-- <input type="text" v-model="task.title"> -->
                <span v-text="task.title"></span>
              </td>
              <td>
                <button class="btn btn-danger" v-on:click="deleteTask(task)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'taskApp',
  components: {},
  data() {
    return {
        nameApp: 'Tasks',
        tasks: [
          {
            title: 'Task 1',
            done: false
          },
          {
            title: 'Task 2',
            done: false
          },
          {
            title: 'Task 3',
            done: false
          }
        ],
        newTask: {
          title: '',
          done: false
        },
        showMessage: false,
        typeMessage: '',
        message: ''
      }
  },
  methods: {
        addTask: function(e) {
          e.preventDefault();
          if (this.newTask.title.trim() == '') {
            this.$refs.newTask.focus();
            this.showAlert('warning');
          } else {
            this.tasks.push({
            title: this.newTask.title,
            done: false
            });
            this.newTask.title = '';
            this.$refs.newTask.focus();
            this.showAlert('success');
          }
        },

        deleteTask: function(task) {
          let position = this.tasks.indexOf(task);
          this.tasks.splice(position, 1);
          this.showAlert('delete');
        },

        showAlert(type) {
          this.showMessage = false;
          if (type == 'success') {
            this.showMessage = true;
            this.typeMessage = 'success';
            this.message = 'Task added successfully';
          } else if (type == 'delete') {
            this.showMessage = true;
            this.typeMessage = 'delete';
            this.message = 'Task deleted successfully';
          } else {
            this.showMessage = true;
            this.typeMessage = 'warning';
            this.message = 'Task can not be empty';
          }

          setTimeout(() => {
            this.showMessage = false;
          }, 3000);
            
        }
      }
}
</script>

<style>
  body {
    padding-top: 5vw;
    background-color: #00001b !important;
  }

  .ml-1 {
    margin: 25px 0;
  }
</style>
