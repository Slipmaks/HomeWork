<template>
  <div>
    <h1>Todo</h1>
    <div>
      <div>
        <h2>Создать новый список</h2>
        <button @click="showListNameInput">Добавить</button>
      </div>

      <div v-if="stateListNameInput">
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
      </div>
    </div>
    <div v-if="list.length">
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
    </div>
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
      isComplete: true,
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
      const currentItem = {
        title: this.title,
        complete: false,
      };
      this.list = [...this.list, currentItem];
      this.title = "";
    },
    deleteItemFromList(itemToRemove) {
      this.list = this.list.filter((i) => i !== itemToRemove);
    },
    completeTask(index, item) {
      if (item.complete == true) {
        this.list[index].complete = false;
        console.log(this.list[index].complete);
        return;
      }

      this.list[index].complete = true;
      console.log(this.list[index].complete);
      return;
    },
  },
  watch: {
    list() {
      localStorage.setItem("item-list", JSON.stringify(this.list));
    },
  },
};
</script>

<style>
.complete {
  text-decoration: line-through;
}
</style>
