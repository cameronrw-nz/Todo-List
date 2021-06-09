<template>
  <div class="container">
    <form @submit.prevent="addItem">
      <input type="text" placeholder="Enter an Item..." v-model="todoItem">
    </form>
    <ul>
      <li
        v-for="(todoItem, todoItemIndex) in todo.todoItems"
        v-bind:key="todoItemIndex"
        v-bind:class="[todoItem.isComplete ? 'clicked' : '']"
        v-on:click="completeItem(todoItemIndex)"
        v-touch="{
            left: () => removeItem(todoItemIndex),
            right: () => swipe('Right'),
            }"
      >
        <div class="itemText">{{todoItem.content}}</div>
        <div
          class="itemClear"
          v-bind:class="[todoItem.isComplete ? 'clicked' : '']"
          v-on:click="removeItem(todoItemIndex)"
        >X</div>
      </li>
    </ul>
    <ul 
        v-for="(completedItem) in todo.completedItems"
        v-bind:key="completedItem.date"
    >
        <h2>{{completedItem.date}}</h2>
      <li
        v-for="(todoItem, todoItemIndex) in completedItem.items"
        v-bind:key="todoItemIndex"
        v-bind:class="[todoItem.isComplete ? 'clicked' : '']"
        v-on:click="completeItem(todoItemIndex, completedItem.date)"
        v-touch="{
            left: () => removeItem(todoItemIndex, completedItem.date),
            right: () => swipe('Right'),
            }"
      >
        <div class="itemText">{{todoItem.content}}</div>
        <div
          class="itemClear"
          v-bind:class="[todoItem.isComplete ? 'clicked' : '']"
          v-on:click="removeItem(todoItemIndex, completedItem.date)"
        >X</div>
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
      todo: {todoItems: []}
    };
  },
  created: function() {
    this.todo = JSON.parse(window.localStorage.getItem("todoItems")) || [];
  },
  methods: {
    addItem() {
      if (this.todoItem.length === 0) {
        return;
      }
      const todo =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
      todo.todoItems.push({
        content: this.todoItem
      });

      this.todoItem = "";

      window.localStorage.setItem("todoItems", JSON.stringify(todo));
      this.todo =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
    },

    removeItem(index, completedAt) {
      let todo =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
        if (completedAt) {
            todo.completedItems.forEach(completed => {
                if (completed.date ===completedAt) {
                    completed.items.splice(index, 1);
                }
            })
        }
        else {
            todo.todoItems.splice(index, 1)
        }

      window.localStorage.setItem("todoItems", JSON.stringify(todo));
      this.todo =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
    },

    completeItem(index, completedAt) {
      let todo =
        JSON.parse(window.localStorage.getItem("todoItems")) || [];
        if (completedAt) {
            todo.completedItems.forEach(completed => {
                if (completed.date ===completedAt) {
                    const item = completed.items.splice(index, 1)[0]

                    item.isComplete = false
                    item.completedAt = undefined

                    todo.todoItems.push(item)
                }
            })
        }
        else {
            const item = todo.todoItems.splice(index, 1)[0];
            item.isComplete = true;
            item.completedAt = new Date().toISOString();

            const date = new Date();
            const options = { year: 'numeric', month: 'numeric', day: 'numeric' };
            const newCompletedAt = date.toLocaleDateString("en-NZ", options);

            const completed = todo.completedItems.find(comp => newCompletedAt === comp.date);
            if (completed) {
                completed.items.push(item)
            }
            else {
                todo.completedItems.splice(0, 0, {date: newCompletedAt, items: [item]})
            }
        }

      
      window.localStorage.setItem("todoItems", JSON.stringify(todo));
      this.todo =
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
  overflow: auto;
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

h2 {
    color: white;
}

.clicked {
  background-color: darkseagreen;
  border-left-color: teal;
}

.itemClear.clicked {
  color: darkseagreen;
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
