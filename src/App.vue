<script>
  export default{
    data() {
      return {
        items: [],
        done: false,
        newToDo: null,
      }
    },
    mounted(){
      if (localStorage.getItem('items')) {
        try {
          this.items = JSON.parse(localStorage.getItem('items'))
        } catch(e) {
          localStorage.removeItem('items')
        }
      }
    },
    methods: {
      addToDo(){
        if (this.newToDo.trim().length > 0) {
          this.items.push({
            message: this.newToDo,
            done: false
          })
          this.newToDo = ''
          this.saveToDo()
        }
      },
      changeDone(item){
        item.done = !item.done
      },
      // ToDo: Сделать редактирование, а потом разбить на компоненты и на гитхаб
      deleteNote(index){
        this.items.splice(index, 1)
        this.saveToDo()
      },
      deleteAll(){
        this.items = []
        this.saveToDo()
      },
      deleteDone(){
        this.items = this.items.filter(item => !item.done)
        this.saveToDo()
      },
      saveToDo(){
        const parse = JSON.stringify(this.items)
        localStorage.setItem('items', parse)
      }
    },
  }
</script>

<template>
  <div class="page">
    <div class="wrapper">
        <h1>ToDo List</h1>
        <div class="addBlock">
          <input placeholder="Add Task" class="todoField" type="text" @keyup.enter="addToDo" v-model="newToDo">
          <button class="addButton" @click="addToDo">Create</button>
        </div>
      <ul>
        <li class="item" v-for="(item, index) in items" :key="index">
          <span :class="{ doned: item.done }" @click="changeDone(item)">{{ item.message }}</span>
          <img class="delete" src="./assets/delete-button.svg" v-on:click="deleteNote(index)">
        </li>
      </ul>
      <button @click="deleteDone" class="deleteButton">Delete completed</button>
      <button @click="deleteAll" class="deleteButton">Delete all</button>
    </div>
  </div>
</template>

<style>
  *{
    font-family: Roboto;
    font-size: 16px;
  }
  .page {
    background: #f1f5f9;
    display: flex;
    justify-content: center;
    width: 100%;
    min-height: 100vh;
    padding-top: 2rem;
  }
  .wrapper {
    width: 900px;
    margin: 0 auto;
  }
  h1{
    font-size: 48px;
    color: black;
    margin-bottom: 2rem;
    font-weight: 600;
  }
  .todoField{
    width: 92%;
    border: 1px solid #E7ECED;
    border-right: none;
    border-radius: 5px;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    padding: 6px 5px;
  }
  .todoField:focus{
    border: 1px solid rgba(3, 168, 244, 0.5);;
    outline: none;
  }
  ::placeholder{
    color: #c5c8c9;
  }
  .addButton{
    background-color: rgba(3, 168, 244, 0.5);
    border: none;
    cursor: pointer;
    border-radius: 5px;
    padding: 7px 12px;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
  .doned{
    text-decoration: line-through;
  }
  ul{
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  #checkbox{
    vertical-align: middle;
  }
  .item{
    position: relative;
    word-wrap: break-word;
    width: 100%;
    padding: 5px 10px;
    margin-top: 10px;
    min-height: 34px;
    background-color: #fff;
  }
  span{
    cursor: pointer;
  }
  .delete{
    position: absolute;
    right: 15px; 
    width: 24px;
    height: 24px;
    cursor: pointer;
  }
  .deleteButton{
    background-color: rgba(3, 168, 244, 0.5);
    border: none;
    border-radius: 5px;
    padding: 7px 12px;
    margin-top: 20px;
    cursor: pointer;
  }
  .deleteButton:last-child{
    margin-left: 10px;
  }
</style>