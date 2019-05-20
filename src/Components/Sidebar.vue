<template>

  <div class="col-3">
    <div class="sidebar">

      <div class="filter-block">
        <div class="dropdown show">
          <a class="btn btn-secondary dropdown-toggle drop" href="#" role="button" id="dropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Все
          </a>
          <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
            <a class="dropdown-item" href="#">По дате добавления</a>
            <a class="dropdown-item" href="#">В обратном хронологическом порядке</a>
          </div>
        </div>
      </div>

      <div class="list-block">
        <div class="list" v-for="(item,index) in lists" @click="selectId(index)">
          {{item.title}}
        </div>
      </div>

      <div class="add-list-block">
        <input type="text" v-model="newTodoList">
        <button class="btn btn-primary" @click="addNewList">Добавить список</button>
      </div>

    </div>
  </div>

</template>
