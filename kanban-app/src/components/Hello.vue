<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1 style="color:#000 !important;">KANBAN-APP</h1>
        <hr>
        <!-- <div class="col-md-10">
        </div> -->
        <div class="col-md-offset-10 col-md-1">
          <!-- Trigger the modal with a button -->
          <button type="button" style="margin-left: 80%;"class="btn btn-info btn-lg" data-toggle="modal" data-target="#myModal">+ ADD</button>
          <h1></h1>
        </div>
        <!-- BACKLOG -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail" >
                <h1 style="background-color:red;">BACKLOG</h1>

                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'backlog'">
                  <div class="caption">
                    <div>
                      <h3><b>{{ backlog.title }}</b></h3>
                      <hr>
                      <p>Point: {{ backlog.point }} </p>
                      <p>Assigned To: {{ backlog.assignTo }} </p>
                      <!-- <button @click="showDetail()" type="button">Show Detail</button> -->
                      <button @click="showDetail(backlog)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                  <!-- modal DETAIl -->


                </div>

              </div>
            </div>
          </div>
        </div>
        <!-- TODO -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:orange;">TO-DO</h1>
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'todo'">
                  <div class="caption">
                    <div>
                      <h3><b>{{ backlog.title }}</b></h3>
                      <hr>
                      <p>Point: {{ backlog.point }} </p>
                      <p>Assigned To: {{ backlog.assignTo }} </p>
                      <button @click="showDetail(backlog)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DOING -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:#b3f442;">DOING</h1>
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'doing'">
                  <div class="caption">
                    <div>
                      <h3><b>{{ backlog.title }}</b></h3>
                      <hr>
                      <p>Point: {{ backlog.point }} </p>
                      <p>Assigned To: {{ backlog.assignTo }} </p>
                      <button @click="showDetail(backlog)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DONE -->

        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:green;">DONE</h1>
                <div class="thumbnail" v-for="backlog in backlogs" v-if="backlog.status == 'done'">
                  <div class="caption">
                    <div>
                      <h3><b>{{ backlog.title }}</b></h3>
                      <hr>
                      <p>Point: {{ backlog.point }} </p>
                      <p>Assigned To: {{ backlog.assignTo }} </p>
                      <button @click="showDetail(backlog)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
    <!-- Modal Add-->
    <div class="modal fade" id="myModal" role="dialog">
      <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Modal Kanban</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title" style="text-align='left'">Title:</label>
                <input v-model="addTasks.title" type="text" class="form-control" id="title">
              </div>
              <div class="form-group">
                <label for="Description">Description:</label>
                <textarea v-model="addTasks.description" class="form-control" rows="5" id="description"></textarea>
              </div>
              <div class="form-group">
                <label for="Point">Point:</label>
                <input v-model="addTasks.point" type="text" class="form-control" id="point">
              </div>
              <div class="form-group">
                <label for="AssignTo">Assign To:</label>
                <input v-model="addTasks.assignTo" type="text" class="form-control" id="assign">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button @click="addTask" type="button" class="btn btn-default" data-dismiss="modal">Add Task</button>
          </div>
        </div>

      </div>
    </div>

    <!-- modal show detail -->
    <div v-if="currentTask !== null" class="modal fade" id="modalDetail" role="dialog">
      <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content" style="text-align:'left'" >
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Detail Task : {{ currentTask.title }} for {{ currentTask.assignTo }} </h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title" style="text-align='left'">Task Description: {{ currentTask.description }}</label>
                <p> Point : {{ currentTask.point }} </p>
                <p> Status : {{ currentTask.status }}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer" >
            <button v-if="currentTask.status == 'todo'" @click="backlog(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">BACKLOG</button>
            <button @click="removeTask(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DELETE</button>
            <button v-if="currentTask.status == 'doing' || currentTask.status == 'backlog'" @click="todo(currentTask)" type="button" class="btn btn-default" data-dismiss="modal">TO-DO</button>
            <button v-if="currentTask.status == 'todo'  || currentTask.status == 'done'" @click="doing(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DOING</button>
            <button v-if="currentTask.status == 'doing'" @click="done(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DONE</button>

          </div>
        </div>

      </div>
    </div>

  </div>
</template>

<script>
import firebase from 'firebase'

  // Initialize Firebase
const config = {
  apiKey: 'AIzaSyBFeB00_BJThgniQHFGWEDb7iD8F0bhoK8',
  authDomain: 'kanban-project.firebaseapp.com',
  databaseURL: 'https://kanban-project.firebaseio.com',
  projectId: 'kanban-project',
  storageBucket: 'kanban-project.appspot.com',
  messagingSenderId: '596201311478'
}
var firebaseApp = firebase.initializeApp(config)

var tasksdb = firebaseApp.database().ref('kanban-task')

// let tasksdb = database.ref('kanban-task')

export default {
  name: 'hello',
  data () {
    return {
      detailTaskModal: false,
      addTasks: {
        title: '',
        description: '',
        point: '',
        assignTo: '',
        status: 'backlog'
      },
      currentTask: null
    }
  },
  firebase: {
    backlogs: tasksdb
  },
  methods: {
    addTask () {
      // console.log('sukses', this.addTask);
      tasksdb.push(this.addTasks)
      // console.log('hiiiii : ',this.backlogs)
      console.log('hi',tasksdb);
      // var dbRef = firebase.database().ref().child();
      this.addTasks.title = ''
      this.addTasks.description = ''
      this.addTasks.point = ''
      this.addTasks.assignTo = ''

    },
    showDetail (backlog) {
      console.log(backlog['.key']);
      this.currentTask = backlog
    },
    removeTask:function(tasks){
      console.log(tasks['.key']);
      tasksdb.child(tasks['.key']).remove()

    },
    backlog (tasks) {
      var data = tasksdb.child(tasks['.key'])
      data.update({
        "status" : "backlog"
      })
    },
    todo (tasks) {
      var data = tasksdb.child(tasks['.key'])
      data.update({
        "status" : "todo"
      })
    },
    doing (tasks) {
      var data = tasksdb.child(tasks['.key'])
      data.update({
        "status" : "doing"
      })
    },
    done (tasks) {
      var data = tasksdb.child(tasks['.key'])
      data.update({
        "status" : "done"
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.thumbnail {
  background-color: #fcf8e3;
}
h1 {
  margin: 0;
  padding: 10px;
  margin-bottom: 2px;
  color: white;
  font-weight: bold;
}
</style>
