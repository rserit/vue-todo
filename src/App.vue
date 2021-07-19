<template>
  <div id="container">
    <div class="row">
      <div class="col-md-8 offset-md-2 text-center">
        <h3 class="mt-5">Todolist: Sample</h3>
        <hr>

      <div class="row">
        <div class="col-md-6-offset-md-3">
          <input v-model="todoText" type="text">
          <button @click="addList()" class="btn btn-primary">Add</button>
        </div>
      </div>
<hr>
        
      <div v-if="todoList.length > 0" class="todo-container">
          <Todo @removeItem="remove($event)" :todo="item" v-for="item in todoList" :key="item.text" />
      </div>
      <div  v-if="todoList.length == 0" class="alert alert-warning" role="alert">
          There is no item on your list but you can add
      </div>
    <button class="btn btn-success" @click="getList()">Sync</button>

      </div>
    </div>


   
  </div>
</template>

<script>
import Todo from "@/components/Todo"
import axios from "axios"
export default {
  components: {Todo},
  data(){
    return{
      todoList: [],
      todoText: ""  
    }
  },
  methods:{
     async addList(){
        if(this.todoText == "" || this.todoText.length < 3) {
          return alert("What is your goal?");
        }

        const j = { text: this.todoText, suser: "recep" };
        let response =  await axios.post("https://vue-todo-sample-81113-default-rtdb.europe-west1.firebasedatabase.app/todos.json", j);
        console.log(response)
    },
    async getList(){
      let response = await axios.get("https://vue-todo-sample-81113-default-rtdb.europe-west1.firebasedatabase.app/todos.json")
      for(let key in response.data){

          let index = this.todoList.findIndex(i=> { return i.id == key });
          console.log(index);
          if(index != -1) {
            continue;
          }

          let arr = { suser: response.data[key].suser, text: response.data[key].text, id: key }
          this.todoList.push(arr)
      }

      console.table(this.todoList)
    },
    async remove(id){
      let response = await axios.delete("https://vue-todo-sample-81113-default-rtdb.europe-west1.firebasedatabase.app/todos/" +id+".json")
      if(response.status != 200) {
        alert("Error! Http response: " + response.statusText);
        return;
      }

      let index = this.todoList.findIndex(i=>{return i.id == id});
      this.todoList.splice(index, 1);
    }
    
  },
  created(){
       this.getList();
  }
}
</script>