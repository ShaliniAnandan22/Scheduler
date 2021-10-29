<!-- <template>
  <div
    style="height: 500px; width: 500px; border: 1px solid red; position: relative;"
  >
    <VueDraggableResizable
      :w="100"
      :h="100"
      @dragging="onDrag"
      @resizing="onResize"
      :parent="true"
    >
      <p>
        Hello! I'm a flexible component. You can drag me around and you can
        resize me.<br />
        X: {{ x }} / Y: {{ y }} - Width: {{ width }} / Height: {{ height }}
      </p>
    </VueDraggableResizable>
  </div>
</template> -->

<template>
  <div style="margin-top: 20px;">
    <template v-for="(spaceEvents, spaceEventIdx) in eventList">
      <VueDraggableResizable
        @dragging="onDrag"
        @resizing="onResize"
        :parent="true"
        :key="spaceEventIdx"
        :prevent-deactivation="true"
        :handles="['mr', 'ml']"
        class="container"
      >
        <!-- <div
        :key="`sidebar-scope-${spaceEventIdx}`"
        :index="spaceEventIdx"
        class="container"
      > -->
        <template v-for="(events, index) in spaceEvents">
          <div
            :key="`block-${index}`"
            :index="index"
            class="table-header-grid"
            style="min-height: 30px;background-color: darkgray;margin-bottom: 10px;"
          >
            <!-- <draggable
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
          > -->
            <div
              v-for="(event, dayIndex) in events.dayEvents"
              :key="`header-${dayIndex}`"
              class="table-header-item"
              style="cursor: move;"
            >
              {{ event.name }}
            </div>
            <!-- </draggable> -->
          </div>
        </template>
        <!-- </div> -->
      </VueDraggableResizable>
    </template>
  </div>
</template>

<script>
import VueDraggableResizable from "vue-draggable-resizable"

export default {
  components: { VueDraggableResizable },
  data() {
    return {
      width: 0,
      height: 0,
      x: 0,
      y: 0,
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
    onResize(x, y, width, height) {
      this.x = x
      this.y = y
      this.width = width
      this.height = height
    },
    onDrag(x, y) {
      this.x = x
      this.y = y
    },
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
