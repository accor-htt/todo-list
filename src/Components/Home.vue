<template>

  <div class="row">
    <div class="col-3">
      <div class="sidebar">
        <div class="filter-block">
          <div class="dropdown show">
            <a class="btn btn-secondary dropdown-toggle drop" href="#" role="button" id="dropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              {{currentCategory}}
            </a>
            <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
              <a class="dropdown-item" v-for="curr in category" :key="curr.id" @click="selectCategory(curr.status)">{{curr.title}}</a>
            </div>

          </div>
        </div>

        <div class="list-block">
          <div class="list" v-for="(item,index) in ArrayA"  :class="getClass(item)" @click="selectId(index)">
            {{item.title}}
<!--            <button class="btn btn-primary" @click="removeList(index, item.id)">X</button>-->
            <button class="btn btn-primary" data-toggle="modal" data-target="#modalDeleteList" @click="beforeRemoveList(item.title, index, item.id)">X</button>

                          <div class="modal fade" id="modalDeleteList" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
                <div class="modal-dialog modal-dialog-centered" role="document">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="exampleModalLongTitle">Удалить список дел</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      Удалить Список дел “{{deleteList}}”
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Отмена</button>
                      <button type="button" class="btn btn-primary" @click="removeList(deleteIndex, deleteIndex2)">Удалить</button>
                    </div>
                  </div>
                </div>
              </div>



          </div>
        </div>

        <div class="add-list-block">
          <input type="text" v-model="newTodoList">
          <button class="btn btn-primary" data-toggle="modal" data-target="#modalAddList" @click="addNewList">Добавить список</button>

          <div class="modal fade" id="modalAddList" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="ModalLongLongLongTitle">Список дел добавлен</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  Новый список дел "{{newTodoList}}" добавлен
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-secondary" data-dismiss="modal">ОК</button>
                </div>
              </div>
            </div>
          </div>

        </div>

      </div>
    </div>

    <div class="col">
      <div class="todo-list">
        <div class="task-block" v-if="this.ArrayA.length !== 0">
          <h3>{{ArrayA[selectTaskId].title}}</h3>
          <div class="block-task" v-for="(todo,index) in ArrayA[this.selectTaskId].todo">
            <div class="doneCheckBox">
              <input type="checkbox" class="doneTask" v-model="todo.done">
            </div>

            <div class="task">

              <div class="modal fade" id="modalDeleteElement" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
                <div class="modal-dialog modal-dialog-centered" role="document">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="ModalLongTitle">Удалить дело</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      Удалить дело “{{deleteItem}}”
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Отмена</button>
                      <button type="button" class="btn btn-primary"@click="removeElement(index)">Удалить</button>
                    </div>
                  </div>
                </div>
              </div>

              <span class="dot" :class="{important: todo.important}"></span>
              <div class="text">
                <span class="description" v-if="!todo.done">{{todo.name}}</span>
                <del v-else><span class="description">{{todo.name}}</span></del>
                <span class="date">{{todo.date}}</span>
                <span class="time">{{todo.time}}</span>
              </div>
              <button class="btn btn-primary" data-toggle="modal" data-target="#modalDeleteElement" @click="beforeRemoveElement(todo.name, index)">X</button>
            </div>
          </div>

        </div> <!-- End task-block -->
        <div class="task-block-empty" v-else>
          <h3 align="center">Списки задач пустой</h3>
        </div>
        <div class="add-task-block" v-if="this.ArrayA.length !== 0">
          <div class="wrapper">
            <form v-on:submit.prevent="addNewTodo">
              <input type="text" v-model="newTodoText" id="new-todo"
                     placeholder="Например: сделать ТЗ">
              <input type="checkbox" v-model="newImportant">
              <span>Срочное</span>
              <button class="btn btn-primary" data-toggle="modal" data-target="#modalAddElement" >Добавить дело</button>
            </form>

            <div class="modal fade" id="modalAddElement" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true" v-if="this.ArrayA.length !== 0">
              <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="ModalLongLongTitle">Дело добавлено</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body" v-if="this.ArrayA[this.selectTaskId].todo.length !== 0">
                    "{{this.ArrayA[this.selectTaskId].todo[0].name}}" добавлено в "{{this.ArrayA[this.selectTaskId].title}}"
                  </div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">ОК</button>
                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>
      </div> <!-- End todo-list -->
    </div> <!-- End col -->
  </div>
</template>
</template>

<script>

  export default {
    name: 'app',
    data() {
      return {
        newDate: new Date(),
        nameList: '',
        newTodoText: '',
        newTodoList: '',
        newImportant: false,
        selectTaskId: 0,
        currentCategory: 'incomplete',
        deleteList: '',
        deleteIndex: '',
        deleteIndex2: '',
        deleteItem: '',
        deleteItemIndex: '',
        search: '',
        category: [
          {
            id: 1,
            title: 'Исполненные',
            status: 'complete'
          },
          {
            id: 2,
            title: 'Неисполненные',
            status: 'incomplete'
          },
          {
            id: 3,
            title: 'Все',
            status: 'all'
          },

        ],
        lists: [
          {
            id: 0,
            title: 'Планы на лето',
            completed: 1,
            todo: [
              {name: 'Сделать что-то 1', important: false, done: true, date: '01.05.2018', time: '21:00'},
              {name: 'Сделать что-то 2', important: true,  done: true, date: '20.04.2019', time: '14:00'},
            ]
          },
          {
            id: 1,
            title: 'Планы на зиму',
            completed: 1,
            todo: [
              {name: 'Сделать что-то 3', important: true, done: true, date: '01.05.2018', time: '18:30'},
              {name: 'Сделать что-то 4', important: false, done: false, date: '20.04.2019', time: '14:00'},
            ]
          },
          {
            id: 2,
            title: 'Выходные',
            completed: 1,
            todo: [
              {name: 'Купить', important: true, done: true, date: '21.04.2018', time: '15:30'},
              {name: 'Выпить', important: false, done: true, date: '21.04.2018', time: '14:00'},
            ]
          },
          {
            id: 3,
            title: 'Пустая хрень',
            completed: 0,
            todo: [
              {name: 'Купить', important: true, done: true, date: '21.04.2018', time: '14:00'},
            ]
          }
          // computed completed
          // 0 - empty, 1: not completed tasks, 2: completed tasks
        ],
        nextIDTODO: 4
      }
    },
    computed: {

      ArrayA: function() {

        var filters = {
          all: function(todos) {
            return todos;
          },
          complete: function(todos) {
            return todos.filter(function(todo) {
              return todo.completed === 2;
            })
          },
          incomplete: function(todos) {
            return todos.filter(function(todo) {
              // return (todo.completed === 1 && todo.completed === 0);
              return todo.completed === 1;
            })
          }
        }

        for (let i = 0; i < this.lists.length; i++) {

          let countTodoComplete = 0;

          for (let j = 0; j < this.lists[i].todo.length; j++) {

            if (this.lists[i].todo[j].done === true)
              ++countTodoComplete;
          }

          if (this.lists[i].todo.length === 0) {
            this.lists[i].completed = 0;
          }
          else if (countTodoComplete === this.lists[i].todo.length) {
            this.lists[i].completed = 2;
          }
          else {
            this.lists[i].completed = 1;
          }

        }

        // Сортировка в алфавитном порядке
        this.sortList();

        // Сортировка: по дате добавления в обратном хронологическом порядке
        // this.sortItem();
        //

        return filters[this.currentCategory](this.lists);
        // return this.lists;
      }


    },
    methods: {

      getClass(item) {
        return {
          colorStatus: item.completed === 2,
          colorAStatus: item.completed === 1,
          colorBStatus: item.completed === 0
        }
      },

      selectCategory: function(fil) {
        this.currentCategory = fil;
        this.selectTaskId = 0;
      },

      sortList: function() {
        this.lists.sort(function (a, b) {
          return a.title.localeCompare(b.title);
        })
      },

      sortItem: function() {
        this.lists[this.selectTaskId].todo.sort(function(a,b) {
          var regex = /\:/;
          let aa = a.time.replace(regex, "");
          let bb = b.time.replace(regex, "");

          return bb - aa;
        })
      },

      selectId: function(index) {
        console.log("длина" + this.lists.length)
        console.log(index >= this.lists.length)
        if (index >= this.lists.length)
          this.selectTaskId = 0
        else
          this.selectTaskId = index
        console.log("selectID: " + this.selectTaskId)
      },

      addNewTodo: function () {
        if (this.newTodoText !== '') {
          this.ArrayA[this.selectTaskId].todo.unshift({
            name: this.newTodoText,
            important: this.newImportant,
            date: this.newDate.toLocaleDateString(),
            time: this.newDate.getHours()+':'+ this.newDate.getMinutes(),
          });
          this.newTodoText = ''
        }
      },

      addNewList: function() {
        if(this.newTodoList !== ''){
          this.lists.push({
            id: this.nextIDTODO++,
            title: this.newTodoList,
            completed: 0,
            todo: [],
          })
        }
      },


      beforeRemoveElement: function(text,index) {
        this.deleteItem = text;
        this.deleteItemIndex = index;
      },

      removeElement: function (index) {
        this.ArrayA[this.selectTaskId].todo.splice(index, 1)
        $('#modalDeleteElement').modal('hide')
      },

      beforeRemoveList: function(text, index, itemId) {
        this.deleteList = text;
        this.deleteIndex = index;
        this.deleteIndex2 = itemId;
      },

      removeList: function(index, itemId) {
        if (this.currentCategory === 'all') {
          this.ArrayA.splice(index,1)
        }
        else {
          console.log("INDEX: " + index)
          this.ArrayA.splice(index, 1)
          this.lists.splice(itemId, 1)
          // console.log(this.ArrayA)
          console.log("DELETE: " + this.selectTaskId)
        }
        $('#modalDeleteList').modal('hide')
      }
    }
  }
</script>

<style>

  .sidebar {
    height: calc(100vh - 10px);
  }

  .sidebar .filter-block {
    width: 100%;
    height: 50px;
    border: 1px solid black;
  }

  .drop {
    display: block;
    width: 200px;
    height: 40px;
    margin: 3px auto;
  }

  input, .btn {
    border-radius: 15px;
  }

  .sidebar .list-block {
    width: 100%;
    height: 750px;
    border: 1px solid black;
    padding: 5px 0;
    overflow-y: auto;
  }

  .list-block .list {
    width: 100%;
    height: 50px;
    margin-bottom: 2px;
    text-align: center;
    -webkit-transition: 0.5s ease;
  }

  .list-block .list:hover {
    border: 3px solid black;
    transition: 0.3s;
  }

  .add-list-block input {
    display: block;
    margin: 3px auto;
  }

  .add-list-block button {
    display: block;
    margin: 3px auto;
  }

  .todo-list {
    height: calc(100vh - 10px);
  }

  .todo-list .task-block {
    padding: 3px;
    height: 800px;
    border: 1px solid black;
    overflow-y: auto;
  }

  .task-block-empty {
    padding: 3px;
    height: 800px;
    border: 1px solid black;
    overflow-y: auto;
  }

  .task-block h3 {
    text-align: center;
  }

  .task-text {
    width: 90%;
    height: 70px;
    margin: auto;
  }

  .task-text .task {
    width: 90%;
    height: 40px;
    border: 1px solid black;
    margin: 0 auto;
    float: left;
  }

  .wrapper {
    width: 80%;
    height: 50px;
    display: block;
    margin: 10px auto;
  }

  .wrapper .delete-task {
    width: 30px;
    height: 20px;
    float: right;
  }


  .task-text .done-task {
    float:left;
    margin-right: 10px;
    width: 30px;
    height: 40px;
  }

  .add-task-block {
    width: 100%;
  }

  .add-task-block .wrapper {
    margin-right: 5px;
  }


  .dot {
    margin: 5px 5px;
    float: left;
    height: 25px;
    width: 25px;
    background-color: grey;
    border-radius: 50%;
  }
/*  ---------------- */

  .block-task {
    padding: 5px;
    width: 700px;
    height: 50px;
    display: flex;
    margin: 0 auto 5px;
  }

  .block-task .doneCheckBox {
    width: 100px;
    height: 40px;
  }

  .doneCheckBox .doneTask {
    width: 100px;
    height: 39px;
  }

  .block-task .task {
    width: 570px;
    height: 40px;
    border: 1px solid black;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .task .text {
    justify-content: center;
  }

  .task-text {
    border: 1px solid black;
    width: 100px;
    height: 30px;
  }

  .task .text {
    display:flex;
    width: 300px;
    justify-content: space-between;
  }

  .task .btn {
  }

  .important {
    background-color: red;
  }

  .colorStatus {
    background-color: gray;
  }

  .colorAStatus {
    background-color: limegreen;
  }

  .colorBStatus {
    background-color: white;
  }

</style>

