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
      >
        <div class="itemText">{{data.content}}</div>
        <div class="itemClear" v-on:click="removeItem(index)">X</div>
      </li>
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
  grid-template-columns: 100%;
  width: 100%;
  flex-shrink: 1;
  overflow-x: none;
}
li {
  display: flex;
  padding: 10px;
  margin-left: 10px;
  width: calc(100% - 50px);
  border-left: 20px solid indianred;
  background-color: coral;
  font-size: 32px;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  text-overflow: wrap;
}

.itemText {
  flex: 1 1 auto;
  font-weight: bold;
  color: #323333;
}
.itemClear {
  background-color: #444444;
  color: coral;
  font-size: 18px;
  font-weight: bold;
  border-radius: 5px;
  height: 22px;
  padding-top: 4px;
  justify-content: center;
  width: 27px;
  margin: 5px 0px;
}

.itemClear:hover {
  color: #323333;
  background-color: coral;
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
  font-weight: bold;
  font-size: 1.3em;
  color: #323333;
  background-color: indianred;
}

.btnClear:hover {
  background-color: rgb(204, 86, 86);
}
</style>
