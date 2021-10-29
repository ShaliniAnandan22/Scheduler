<template>
  <div style="margin-top: 20px;">
    <template v-for="(spaceEvents, spaceEventIdx) in eventList">
      <div
        :key="`sidebar-scope-${spaceEventIdx}`"
        :index="spaceEventIdx"
        class="container"
      >
        <template v-for="(events, index) in spaceEvents">
          <draggable
            :key="`block-${index}`"
            :list="events.dayEvents"
            @end="(ent) => updateList(ent, 'block', index)"
            @change="handleEvents"
            v-bind="fieldSectionDragOptions"
            :index="index"
            group="block"
            draggable=".table-header-item"
            class="table-header-grid"
            style="min-height: 30px;background-color: darkgray;margin-bottom: 10px;"
          >
            <div
              v-for="(event, dayIndex) in events.dayEvents"
              :key="`header-${dayIndex}`"
              class="table-header-item"
              style="cursor: move;"
            >
              {{ event.name }}
            </div>
          </draggable>
        </template>
      </div>
    </template>
  </div>
</template>

<script>
import draggable from "vuedraggable"

export default {
  name: "hello",
  components: {
    draggable,
  },
  data() {
    return {
      list: [
        { name: "Sunday", startTime: 1631989800000, endTime: 1632076199999 },
        { name: "Monday", startTime: 1632076200000, endTime: 1632162599999 },
        { name: "Tuesday", startTime: 1632162600000, endTime: 1632248999999 },
        { name: "Wednesday", startTime: 1632249000000, endTime: 1632335399999 },
        { name: "Thursday", startTime: 1632335400000, endTime: 1632421799999 },
        { name: "Friday", startTime: 1632421800000, endTime: 1632508199999 },
        { name: "Saturday", startTime: 1632508200000, endTime: 1632594599999 },
      ],
      fieldSectionDragOptions: {
        animation: 150,
        easing: "cubic-bezier(1, 0, 0, 1)",
        group: "tasksection",
        sort: true,
      },
      movedScopeOldIndex: null,
      movedBlockOldIndex: null,
      movedScopeNewIndex: null,
      movedBlockNewIndex: null,
      eventList: [
        [
          {
            startTime: 1631989800000,
            endTime: 1632076199999,
            dayEvents: [
              {
                name: "Floor 1 - Event 3",
                start: 1632027600000,
                end: 1632094200000,
                time: "10:30:00",
                startScope: true,
              },
            ],
          },
          {
            startTime: 1632076200000,
            endTime: 1632162599999,
            dayEvents: [
              {
                name: "Floor 1 - Event 3",
                start: 1632027600000,
                end: 1632094200000,
                time: "10:30:00",
                startScope: false,
              },
              {
                name: "Event 1",
                start: 1632099600000,
                end: 1632313800000,
                time: "06:30:00",
                startScope: true,
              },
              {
                name: "Floor 1 - Event 5",
                start: 1632146400000,
                end: 1632236400000,
                time: "19:30:00",
                startScope: true,
              },
              {
                name: "Floor 1 - Event 4",
                start: 1632159000000,
                end: 1632232800000,
                time: "23:00:00",
                startScope: true,
              },
            ],
          },
          {
            startTime: 1632162600000,
            endTime: 1632248999999,
            dayEvents: [
              {
                name: "Event 1",
                start: 1632099600000,
                end: 1632313800000,
                time: "06:30:00",
                startScope: false,
              },
              {
                name: "Floor 1 - Event 5",
                start: 1632146400000,
                end: 1632236400000,
                time: "19:30:00",
                startScope: false,
              },
              {
                name: "Floor 1 - Event 4",
                start: 1632159000000,
                end: 1632232800000,
                time: "23:00:00",
                startScope: false,
              },
              {
                name: "Test 1",
                start: 1632182400000,
                end: 1632200400000,
                time: "05:30:00",
                startScope: true,
              },
              {
                name: "Floor 1 - Event 2",
                start: 1632212100000,
                end: 1632276000000,
                time: "13:45:00",
                startScope: true,
              },
            ],
          },
          {
            startTime: 1632249000000,
            endTime: 1632335399999,
            dayEvents: [
              {
                name: "Event 1",
                start: 1632099600000,
                end: 1632313800000,
                time: "06:30:00",
                startScope: false,
              },
              {
                name: "Floor 1 - Event 2",
                start: 1632212100000,
                end: 1632276000000,
                time: "13:45:00",
                startScope: false,
              },
              {
                name: "Floor 1 - Event 6",
                start: 1632297600000,
                end: 1632319200000,
                time: "13:30:00",
                startScope: true,
              },
            ],
          },
          { startTime: 1632335400000, endTime: 1632421799999, dayEvents: [] },
          { startTime: 1632421800000, endTime: 1632508199999, dayEvents: [] },
          { startTime: 1632508200000, endTime: 1632594599999, dayEvents: [] },
        ],
        [
          {
            startTime: 1631989800000,
            endTime: 1632076199999,
            dayEvents: [
              {
                name: "Event 2",
                start: 1632011400000,
                end: 1632119400000,
                time: "06:00:00",
                startScope: true,
              },
            ],
          },
          {
            startTime: 1632076200000,
            endTime: 1632162599999,
            dayEvents: [
              {
                name: "Event 2",
                start: 1632011400000,
                end: 1632119400000,
                time: "06:00:00",
                startScope: false,
              },
            ],
          },
          { startTime: 1632162600000, endTime: 1632248999999, dayEvents: [] },
          { startTime: 1632249000000, endTime: 1632335399999, dayEvents: [] },
          { startTime: 1632335400000, endTime: 1632421799999, dayEvents: [] },
          { startTime: 1632421800000, endTime: 1632508199999, dayEvents: [] },
          { startTime: 1632508200000, endTime: 1632594599999, dayEvents: [] },
        ],
        [
          { startTime: 1631989800000, endTime: 1632076199999, dayEvents: [] },
          { startTime: 1632076200000, endTime: 1632162599999, dayEvents: [] },
          { startTime: 1632162600000, endTime: 1632248999999, dayEvents: [] },
          { startTime: 1632249000000, endTime: 1632335399999, dayEvents: [] },
          {
            startTime: 1632335400000,
            endTime: 1632421799999,
            dayEvents: [
              {
                name: "Event 3",
                start: 1632400200000,
                end: 1632594599999,
                time: "18:00:00",
                startScope: true,
              },
            ],
          },
          {
            startTime: 1632421800000,
            endTime: 1632508199999,
            dayEvents: [
              {
                name: "Event 3",
                start: 1632400200000,
                end: 1632594599999,
                time: "18:00:00",
                startScope: false,
              },
            ],
          },
          {
            startTime: 1632508200000,
            endTime: 1632594599999,
            dayEvents: [
              {
                name: "Event 3",
                start: 1632400200000,
                end: 1632594599999,
                time: "18:00:00",
                startScope: false,
              },
            ],
          },
        ],
      ],
    }
  },
  methods: {
    // orderList() {
    //   this.list = this.list.sort((one, two) => {
    //     return one.order - two.order;
    //   });
    // },
    // onMove({ relatedContext, draggedContext }) {
    //   const relatedElement = relatedContext.element
    //   const draggedElement = draggedContext.element
    //   return (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
    // },
    updateList(events, scopeName, scopeIdx) {
      let { newDraggableIndex, newIndex, oldDraggableIndex, oldIndex } = events
      console.log(
        scopeName,
        scopeIdx,
        newDraggableIndex,
        newIndex,
        oldDraggableIndex,
        oldIndex,
        events
      )

      let { to, from } = events || {}
      let { __vue__, parentElement: toParentElement } = to || {}
      let { $attrs } = __vue__ || {}
      let { index } = $attrs
      console.log(scopeName, index)

      let { parentElement: fromParentElement } = from || {}
      // let { __vue__: fromParentVue } = fromParentElement || {}
      // let { $attrs: fromParentAttrs } = fromParentVue || {}
      // let { index: fromParentIdx } = fromParentAttrs
      let { attributes: fromParentAttributes } = fromParentElement || {}
      // let { value: fromParentIdx } = (fromParentAttributes || []).find(
      //   (att) => att.name === "index"
      // )
      let fromParentIdx = parseInt(fromParentAttributes.index.value)
      console.log("sidebar-scope-from-index", fromParentIdx)

      // let { __vue__: toParentVue } = toParentElement || {}
      // let { $attrs: toParentAttrs } = toParentVue || {}
      // let { index: toParentIdx } = toParentAttrs
      let { attributes: toParentAttributes } = toParentElement || {}
      // let { value: toParentIdx } = (toParentAttributes || []).find(
      //   (att) => att.name === "index"
      // )
      let toParentIdx = parseInt(toParentAttributes.index.value)
      console.log("sidebar-scope-to-index", toParentIdx)
    },
    handleEvents(events) {
      console.log("handle-events", events)
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
      }
    },
    // listString() {
    //   return JSON.stringify(this.list, null, 2)
    // },
    // list2String() {
    //   return JSON.stringify(this.list2, null, 2)
    // },
  },
  watch: {
    // isDragging(newValue) {
    //   if (newValue) {
    //     this.delayedDragging = true
    //     return
    //   }
    //   this.$nextTick(() => {
    //     this.delayedDragging = false
    //   })
    // },
  },
}
</script>
<style scoped>
.table-header-grid {
  display: flex;
  justify-content: space-between;
  width: 100%;
  overflow: scroll;
  margin-right: 5px;
}
.table-header-item {
  min-width: 100px;
  width: 100%;
  border: 1px solid;
  border-left: none;
}
.table-header-item:first-child {
  border-left: 1px solid;
}
.container {
  display: flex;
}
</style>
