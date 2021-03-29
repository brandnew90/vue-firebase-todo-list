<template>
  <div class="todo">
    <h1>ToDo List</h1>
    <h3 v-if="items.length == 0">No hay tareas pendientes</h3>
    <ul v-else>
      <li v-for="(item, index) in items" :key="index">
        <input
          type="checkbox"
          :value="index"
          v-model="item.checked"
          @click="onClickCheckbox(item)"
        />
        {{ item.name }}
        <a href="#" style="color: blue" @click="deleteItem(item)">Borrar</a>
      </li>
    </ul>
    <div>
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
li {
  margin: 0px;
  padding: 10px;
}
ul {
  padding-inline-start: 0px;
  text-align: center;
}
pre {
  display: inline-block;
}
</style>