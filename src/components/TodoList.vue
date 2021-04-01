<template>
  <div class="todo-list">
    <h1 class="center">Lista de tareas</h1>
    <h3 class="center" v-if="items.length == 0">No hay tareas pendientes</h3>
    <ul class="center" v-else>
      <li v-for="(item, index) in items" :key="index">
        <input
          type="checkbox"
          v-model="item.checked"
          @click="onClickCheckbox(item)"
        />
        {{ item.name }}
        <a href="#" @click="deleteItem(item)">Borrar</a>
      </li>
    </ul>
    <div class="center">
      <h3>Agregar nuevo:</h3>
      <input type="text" placeholder="Escribe aquí..." v-model="newItemName" />
      <input type="button" value="Agregar" @click="addItem" />
    </div>
  </div>
</template>

<script>
import { db } from "../db";

export default {
  name: "TodoList",
  data() {
    return {
      newItemName: "",
      items: [],
    };
  },
  firebase: {
    items: db.ref("items"),
  },

  methods: {
    deleteItem: function (item) {
      db.ref("items/" + item[".key"]).remove();
    },
    addItem: function () {
      const newItem = { name: this.newItemName, checked: false };
      db.ref("items").push(newItem);
      this.newItemName = "";
    },
    onClickCheckbox: function (item) {
      const copyItem = item;
      copyItem.checked = !copyItem.checked;
      db.ref("items/" + item[".key"]).set(copyItem);
    },
  },
};
</script>

<style>
.todo-list {
  /* width: 40%;
  margin: 0 auto; */
  float: left;
  position: relative;
  left: 50%;
}

.center {
  left: -50%;
  position: relative;
}
li {
  margin: 0px;
  padding: 5px;
  text-align: left;
}
ul {
  padding-inline-start: 0px;
  /* text-align: center; */
  list-style-type: none;

  background-color: bisque;
  padding: 20px;
}
</style>