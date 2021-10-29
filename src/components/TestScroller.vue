<template>
  <div id="app">
    <div class="wrapper">
      <virtual-list
        class="list"
        style="height: 360px; overflow-y: auto;"
        :data-key="'id'"
        :data-sources="items"
        :estimate-size="50"
      >
        <div class="item">
          <div class="id">{{ source.id }} - {{ source.text }}</div>
        </div>
      </virtual-list>
    </div>
  </div>
</template>

<script>
// import Item from './Item'
// import { getData } from "./data"
import VirtualList from "vue-virtual-scroll-list"

export default {
  name: "App",
  components: { VirtualList },
  data() {
    return {
      //   item: Item,
      items: this.getData(1000),
    }
  },
  methods: {
    getData(count) {
      let idCounter = 0
      const data = []
      for (let index = 0; index < count; index++) {
        data.push({
          id: String(idCounter++),
          text: Math.random()
            .toString(16)
            .substr(10),
        })
      }
      return data
    },
  },
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 1em;
  padding: 1em;
}
.list {
  border: 2px solid steelblue;
  border-radius: 3px;
}
</style>
