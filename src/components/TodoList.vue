<template>
  <div class="container">
    <form @submit.prevent="addItem">
      <input type="text" placeholder="Enter an Item..." v-model="todoItem">
    </form>
    <ul>
      <li
        v-for="(data, index) in todoItems"
        v-bind:key="index"
        v-touch="{
      left: () => removeItem(index),
      right: () => swipe('Right'),
    }"
      >{{data.content}}</li>
    </ul>

    <div class="empty"/>

    <div class="btnClear" v-on:click="clearAll">{{"Clear All"}}</div>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      todoItem: "",
      todoItems: []
    };
  },
  created: function() {
    this.todoItems = JSON.parse(window.localStorage.getItem("todoItems")) || [];
  },
  methods: {
    addItem() {
      if (this.todoItem.length === 0) {
        return;
      }
      const todoItems =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
      todoItems.push({
        content: this.todoItem
      });

      this.todoItem = "";

      window.localStorage.setItem("todoItems", JSON.stringify(todoItems));
      this.todoItems =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
    },
    removeItem(index) {
      let todoItems =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
      todoItems.splice(index, 1);

      window.localStorage.setItem("todoItems", JSON.stringify(todoItems));
      this.todoItems =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
    },
    clearAll() {
      window.localStorage.clear("todoItems");
      this.todoItems =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  width: 90%;
  display: flex;
  flex-direction: column;
}
ul {
  list-style-type: none;
  padding: 0;
  margin-top: 10px;
  display: grid;
  grid-row-gap: 5px;
  width: 100%;
  flex-shrink: 1;
  overflow-y: auto;
}
ul li {
  display: inline-block;
  padding: 10px;
  margin-left: 10px;
  width: calc(100% - 50px);
  border-left: 20px solid indianred;
  background-color: coral;
  font-size: 30px;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
}

input {
  flex: 0 0 auto;
  border: 0;
  width: calc(100% - 60px);
  padding: 20px 30px;
  font-size: 40px;
  background-color: #323333;
  color: indianred;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

.empty {
  flex-grow: 1;
  flex-shrink: 1;
}

.btnClear {
  flex: 0 0 auto;
  width: calc(100% -40px);
  margin: 10px 0px;
  border-radius: 10px;
  padding: 10px;
  font-size: 1.3em;
  background-color: indianred;
}
</style>
