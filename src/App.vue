<template>
  <div>
    <h1>Todo</h1>
    <div>
      <div>
        <h2>Создать новый список</h2>
        <button @click="showListNameInput">Добавить</button>
      </div>

      <template v-if="stateListNameInput">
        <hr />
        <input
          v-model="title"
          placeholder="text me"
          @keydown.enter="addItemToList"
        />
        <button @click="addItemToList">Добавить</button>
        <button @click="showListNameInput">Отмена</button>
        <div>
          <h2>Имя cписка:</h2>
          <h3>{{ title }}</h3>
        </div>
      </template>
    </div>
    <template v-if="list.length">
      <ul>
        <hr />
        <li
          v-for="(item, index) in list"
          :key="item.id"
          :class="{ complete: item.complete }"
        >
          {{ index + 1 }} {{ item.title }}
          <button @click.stop="deleteItemFromList(item)">X</button>
          <button @click="completeTask(index, item)">V</button>
        </li>
      </ul>
      <button @click="clearList">Clear list</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      stateListNameInput: false,
      id: "",
      title: "",
      list: [],
    };
  },
  created() {
    const listData = localStorage.getItem("item-list");
    if (listData) {
      this.list = JSON.parse(listData);
    }
  },

  methods: {
    showListNameInput() {
      this.stateListNameInput = !this.stateListNameInput;
    },
    addItemToList() {
      this.id = this.list.length + 1;
      const currentItem = {
        id: this.id,
        title: this.title,
        complete: false,
      };
      this.list = [...this.list, currentItem];
      this.title = "";
    },
    deleteItemFromList(itemToRemove) {
      this.list = this.list.filter((i) => i !== itemToRemove);
      this.list.forEach((item, idx) => (item.id = idx + 1));
    },
    completeTask(index, item) {
      if (item.complete == true) {
        this.list[index].complete = false;
        this.updateListData();
        return;
      }
      this.list[index].complete = true;
      this.updateListData();
    },
    updateListData() {
      localStorage.setItem("item-list", JSON.stringify(this.list));
    },
    clearList() {
      this.list = [];
    },
  },
  watch: {
    list() {
      this.updateListData();
    },
  },
};
</script>

<style>
.complete {
  text-decoration: line-through;
}
</style>
