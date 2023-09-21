<script setup>
import TheWelcome from "../components/TheWelcome.vue";
</script>

<script>
import store from "../store";
export default {
  data() {
    return {
      users: [],

      edit_userid: '',

      edit_user: "",
      edit_number: "",
      edit_age: "",
      edit_img: "",

      inputVisibility: false,



      newName: '',
      newNumber: '',
      newAge: '',
      newPhoto: '',
      

      
    };
  },
  mounted() {
    this.users = store.getters.users;
  },
  methods: {
    valid(){
      let inps = document.querySelectorAll('.cre')
    for(let i of inps){
      i.style.border = '2px solid black'
    }
    },
    create(){
      let inps = document.querySelectorAll('.cre')
      if(this.newName !== '' && this.newPhoto !== '' && this.newNumber !== '' && this.newAge !== ''){
          for(let i of inps){
           i.style.border = '2px solid lightgreen'
          }
          
        
        let obj = {
          id:  Math.random().toString().substr(2,6),
          username: this.newName,
          number:  this.newNumber,
          age: this.newAge,
          img: this.newPhoto,
        }
        
        
        store.dispatch("ADD_USER", obj)
      }else{
          
          for(let i of inps){
           i.style.border = '2px solid #FF3300'
          }
      }

    },


    editItem(item){
      this.inputVisibility = !this.inputVisibility
      this.edit_userid = item.id
      this.edit_user =  item.username
      this.edit_number =  item.number
      this.edit_age =  item.age
      this.edit_img =  item.img
    },


    edit() {
      let obj = {
        id:  this.edit_userid,
        username: this.edit_user,
        number:  this.edit_number,
        age: this.edit_age,
        img: this.edit_img,
      }

     
      store.dispatch("EDIT_USER", obj)
    },



    inp() {
      if (event.target.id == 0) {
        this.edit_user = event.target.value;
      } else if (event.target.id == 1) {
        this.edit_age = event.target.value;
      } else {
        this.edit_number = event.target.value;
      }
    },
    hideInput(item) {
      this.inputVisibility = !this.inputVisibility;
    },
  },
};
</script>

<template>
  <main @click="valid()">
    <!-- create -->
    <div class="header">

      <form @submit.prevent="create()" class="edit">
        <input class="cre" id="0" v-model="newName" type="text" placeholder="Name:" />
        <input class="cre" id="1" v-model="newAge" type="text" placeholder="Age:" />
        <input class="cre" id="2" v-model="newNumber" type="text" placeholder="Phone:" />
        <input class="cre" id="2" v-model="newPhoto" type="text" placeholder="url" />
        <button>Apply</button>
      </form>
      
    </div>
    
    <!-- blocks -->
    <section class="wrapper">
      <div
        class="blocks"
        v-for="item of this.users"
        :id="item.id"
        :key="item.id"
      >
        <img width="180" :src="item.img" />
        <h1>{{ item.username }}</h1>
        <p>Age:{{ item.age }}</p>
        <p>Phone:{{ item.number }}</p>
        <button @click="editItem(item)">Edit</button>
      </div>
    </section>
    



    
  <div class="efit"  v-if="this.inputVisibility">
      <!-- edit -->
      <form @submit.prevent="edit()" v-if="this.inputVisibility" class="edit">
      <h1>Edit user:{{ this.edit_userid }}</h1>
      <input style="pointer-events: all;" id="0" v-model="edit_user" type="text" placeholder="Name:" />
      <input style="pointer-events: all;" id="1" v-model="edit_age" type="text" placeholder="Age:" />
      <input style="pointer-events: all;" id="2" v-model="edit_number" type="text" placeholder="Phone:" />
      <input style="pointer-events: all;" id="2" v-model="edit_img" type="text" placeholder="img:" />
      <button style="pointer-events: all;">Apply</button>
    </form>
  </div>

  </main>
</template>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
::-webkit-scrollbar{
  appearance: none;
  display: none; 
}
img {
  width: 200px !important;
  border-radius: 10px;
}

.edit {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}
.wrapper{
  width: 100%;
  height: 100vh;
  flex-wrap: wrap;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-between;
}
input{
  padding-left: 15px;
  background: #333;
  border: 2px solid rgb(0, 0, 0);
}
button{
  cursor: pointer;
}
input:focus{
  outline: none;
}
.header{
  width: 100%;
  height: 70px;
  padding: 0px 100px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
@keyframes an {
  0%{
    bottom: -100%;
  }
  100%{
    bottom: 0;
  }
}
.efit{
  width: 100%;
  pointer-events: none;
  backdrop-filter: blur(10px);
  height: 100px;
  padding: 0px 100px;
  position: fixed;
  bottom: 0;
  transition: .3s;
  display: flex;
  animation: an .2s;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
input {
  width: 180px;
  height: 50px;
  border-radius: 5px;
}
section {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.blocks {
  height: 400px;
  border: 2px solid rgb(0, 0, 0);
  width: 300px;
  margin: 20px;
  padding: 0px 0px 10px 0px;
  border-radius: 20px;
  background: #1d1d1d;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
}
button {
  padding: 10px 55px;
  border-radius: 5px;
  background-color: rgb(52, 52, 248);
  border: 2px solid rgb(52, 52, 248);
  color: aliceblue;
}
</style>