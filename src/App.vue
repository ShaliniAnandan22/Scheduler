<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <!-- <Calendar></Calendar> -->
    <!-- <VirtualScroll></VirtualScroll> -->
    <!-- <AnotherApproach></AnotherApproach> -->
    <!-- <DraggableResize></DraggableResize> -->
    <!-- <Approach3></Approach3> -->
    <!-- <DraggableComponent></DraggableComponent> -->
    <!-- <TestScroller></TestScroller> -->
    <!-- <Approach4></Approach4> -->
    <!-- <Approach5></Approach5> -->
    <HtmlDragEvents></HtmlDragEvents>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
// import Calendar from "./components/MyComponent.vue"
// import VirtualScroll from "./components/VirtualScroll.vue"
// import AnotherApproach from "./components/AnotherApproach.vue"
// import Approach3 from "./components/Approach3"
// import DraggableComponent from "./components/DraggableComponent"
// import TestScroller from "./components/TestScroller.vue"
// import DraggableResize from "./components/DraggableResize.vue"
// import Approach4 from "./components/Approach4.vue"
// import Approach5 from "./components/Approach5.vue"
import HtmlDragEvents from "./components/Approch6DragEvents"

export default {
  name: "App",
  components: {
    // HelloWorld
    // Calendar,
    // VirtualScroll,
    // AnotherApproach,
    // Approach3,
    // DraggableComponent,
    // TestScroller,
    // DraggableResize,
    // Approach4,
    // Approach5,
    HtmlDragEvents,
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
