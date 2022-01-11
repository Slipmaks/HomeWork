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
        <input v-model="title" placeholder="text me" />
        <button @click="createNewList">Добавить</button>
        <button @click="showListNameInput">Отмена</button>
        <div>
          <h2>Имя списка:</h2>
          <h3>{{ title }}</h3>
        </div>
      </div>
    </div>
    <div v-if="list.length">
      <ul>
        <hr />
        <li v-for="(item, index) in list" :key="item">
          {{ index + 1 }} {{ item.title }}
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
      title: "",
      list: [
        {
          title: "some title",
          complete: false,
        },

        {
          title: "quis ut nam facilis et officia qui",
          completed: false,
        },
      ],
    };
  },

  // mounted() {
  //   const placeholderJson = fetch(
  //     "https://jsonplaceholder.typicode.com/todos/1"
  //   ).then((r) => r.json());
  // },
  methods: {
    showListNameInput() {
      this.stateListNameInput = !this.stateListNameInput;
    },
    createNewList() {
      const currentItem = {
        title: this.title,
        complete: false,
      };
      this.list = [...this.list, currentItem];
      this.title = "";
    },
  },
};
</script>

<style></style>
