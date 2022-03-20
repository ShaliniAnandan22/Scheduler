<template>
  <div class="grid-container">
    <div class="flex">
      <div class="grid-row-fixed grid-row">
        <div class="grid-item-sidebar grid-item">Space</div>
        <div
          v-for="(dayName, index) in weekDays"
          :key="`sidebar-${index}`"
          class="grid-item"
        >
          {{ dayName.name }}
        </div>
      </div>
      <template v-for="(rowEventDetails, name) in eventListArray">
        <template v-for="rowEvent in rowEventDetails">
          <template v-if="rowEvent">
            <div
              :index="rowEvent.index"
              :class="[
                'position-relative grid-row',
                name === 'pinnedRow' ? 'pinned-grid' : '',
              ]"
              :key="rowEvent.index"
            >
              <div class="grid-item-sidebar grid-item">
                {{ rowEvent.sidebarScope }}
              </div>
              <div
                class="position-relative grid-row"
                @mousemove.stop="handleEvents($event, 'mousemove', {})"
                @mouseup.stop="handleEvents($event, 'mouseup', {})"
              >
                <div
                  v-for="(event, eventIdx) in rowEvent.eventList"
                  :key="`day-event-${eventIdx}`"
                  class="grid-item"
                  @click="blockClicked(rowEvent.sidebarScope, eventIdx)"
                  v-bind="
                    bindElementDetails(
                      eventIdx,
                      rowEvent.sidebarScope,
                      rowEvent.index
                    )
                  "
                  :style="getBlockHeight(rowEvent.sidebarScope)"
                >
                  <template v-if="event.dayEvents.length">
                    <div
                      v-for="(eventItem, eventItemIdx) in event.dayEvents"
                      :key="`${eventItem.name}-${eventItemIdx}`"
                      :class="applyClass(eventItem, rowEvent)"
                      :style="
                        calculateStyle(
                          eventItem,
                          eventIdx,
                          rowEvent.sidebarScope
                        )
                      "
                      v-bind="
                        bindElementDetails(
                          eventIdx,
                          rowEvent.sidebarScope,
                          rowEvent.index,
                          eventItem.name
                        )
                      "
                      @click="eventClicked(eventItem)"
                    >
                      <div
                        class="text-overflow-ellipsis event-resize"
                        v-bind="
                          bindElementDetails(
                            eventIdx,
                            rowEvent.sidebarScope,
                            rowEvent.index,
                            eventItem.name
                          )
                        "
                      >
                        <div
                          class="resize-div"
                          v-bind="
                            bindElementDetails(
                              eventIdx,
                              rowEvent.sidebarScope,
                              rowEvent.index,
                              eventItem.name,
                              'start'
                            )
                          "
                          @mousedown.stop="
                            handleEvents($event, 'mousedown-resize', eventItem)
                          "
                        ></div>
                        <div
                          class="event-item-class"
                          v-bind="
                            bindElementDetails(
                              eventIdx,
                              rowEvent.sidebarScope,
                              rowEvent.index,
                              eventItem.name
                            )
                          "
                        >
                          <!-- Time -->
                          <!-- <span style="padding-right: 5px;" v-bind="
                                  bindElementDetails(
                                    eventIdx,
                                    rowEvent.sidebarScope,
                                    rowEvent.index,
                                    eventItem.name
                                  )
                                ">
                            {{ eventItem.startScope ? eventItem.time : null }}
                          </span> -->
                          <!-- Event Name -->
                          <span
                            v-bind="
                              bindElementDetails(
                                eventIdx,
                                rowEvent.sidebarScope,
                                rowEvent.index,
                                eventItem.name
                              )
                            "
                          >
                            {{ eventItem.startScope ? eventItem.name : null }}
                          </span>
                        </div>
                        <div
                          class="resize-div"
                          v-bind="
                            bindElementDetails(
                              eventIdx,
                              rowEvent.sidebarScope,
                              rowEvent.index,
                              eventItem.name,
                              'end'
                            )
                          "
                          @mousedown.stop="
                            handleEvents($event, 'mousedown-resize', eventItem)
                          "
                        ></div>
                      </div>
                    </div>
                  </template>
                </div>
              </div>
            </div>
          </template>
        </template>
      </template>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dayTime: 86400000,
      weekDays: [
        {
          name: "Sunday",
          date: 19,
          startTime: 1631989800000,
          endTime: 1632076199999,
        },
        {
          name: "Monday",
          date: 20,
          startTime: 1632076200000,
          endTime: 1632162599999,
        },
        {
          name: "Tuesday",
          date: 21,
          startTime: 1632162600000,
          endTime: 1632248999999,
        },
        {
          name: "Wednesday",
          date: 22,
          startTime: 1632249000000,
          endTime: 1632335399999,
        },
        {
          name: "Thursday",
          date: 23,
          startTime: 1632335400000,
          endTime: 1632421799999,
        },
        {
          name: "Friday",
          date: 24,
          startTime: 1632421800000,
          endTime: 1632508199999,
        },
        {
          name: "Saturday",
          date: 25,
          startTime: 1632508200000,
          endTime: 1632594599999,
        },
      ],
      sidebarList: [
        { name: "Floor 1", pinned: false },
        { name: "Floor 2", pinned: false },
        { name: "Floor 3", pinned: false },
        { name: "Floor 4", pinned: false },
        { name: "Floor 5", pinned: false },
        { name: "Yem", pinned: false },
      ],
      eventListArray: {
        pinnedRow: null,
        eventList: [
          {
            eventList: [
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
              {
                startTime: 1632335400000,
                endTime: 1632421799999,
                dayEvents: [],
              },
              {
                startTime: 1632421800000,
                endTime: 1632508199999,
                dayEvents: [],
              },
              {
                startTime: 1632508200000,
                endTime: 1632594599999,
                dayEvents: [],
              },
            ],
            index: 0,
            sidebarScope: "Floor 1",
          },
          {
            eventList: [
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
              {
                startTime: 1632162600000,
                endTime: 1632248999999,
                dayEvents: [
                  {
                    name: "Floor 2 - Event 2",
                    start: 1632200400000,
                    end: 1632207600000,
                    time: "10:30:00",
                    startScope: true,
                  },
                ],
              },
              {
                startTime: 1632249000000,
                endTime: 1632335399999,
                dayEvents: [],
              },
              {
                startTime: 1632335400000,
                endTime: 1632421799999,
                dayEvents: [],
              },
              {
                startTime: 1632421800000,
                endTime: 1632508199999,
                dayEvents: [],
              },
              {
                startTime: 1632508200000,
                endTime: 1632594599999,
                dayEvents: [],
              },
            ],
            index: 1,
            sidebarScope: "Floor 2",
          },
          {
            eventList: [
              {
                startTime: 1631989800000,
                endTime: 1632076199999,
                dayEvents: [],
              },
              {
                startTime: 1632076200000,
                endTime: 1632162599999,
                dayEvents: [],
              },
              {
                startTime: 1632162600000,
                endTime: 1632248999999,
                dayEvents: [],
              },
              {
                startTime: 1632249000000,
                endTime: 1632335399999,
                dayEvents: [],
              },
              {
                startTime: 1632335400000,
                endTime: 1632421799999,
                dayEvents: [],
              },
              {
                startTime: 1632421800000,
                endTime: 1632508199999,
                dayEvents: [],
              },
              {
                startTime: 1632508200000,
                endTime: 1632594599999,
                dayEvents: [],
              },
            ],
            index: 2,
            sidebarScope: "Floor 3",
          },
          {
            eventList: [
              {
                startTime: 1631989800000,
                endTime: 1632076199999,
                dayEvents: [],
              },
              {
                startTime: 1632076200000,
                endTime: 1632162599999,
                dayEvents: [],
              },
              {
                startTime: 1632162600000,
                endTime: 1632248999999,
                dayEvents: [],
              },
              {
                startTime: 1632249000000,
                endTime: 1632335399999,
                dayEvents: [],
              },
              {
                startTime: 1632335400000,
                endTime: 1632421799999,
                dayEvents: [],
              },
              {
                startTime: 1632421800000,
                endTime: 1632508199999,
                dayEvents: [],
              },
              {
                startTime: 1632508200000,
                endTime: 1632594599999,
                dayEvents: [],
              },
            ],
            index: 3,
            sidebarScope: "Floor 4",
          },
          {
            eventList: [
              {
                startTime: 1631989800000,
                endTime: 1632076199999,
                dayEvents: [],
              },
              {
                startTime: 1632076200000,
                endTime: 1632162599999,
                dayEvents: [],
              },
              {
                startTime: 1632162600000,
                endTime: 1632248999999,
                dayEvents: [],
              },
              {
                startTime: 1632249000000,
                endTime: 1632335399999,
                dayEvents: [],
              },
              {
                startTime: 1632335400000,
                endTime: 1632421799999,
                dayEvents: [],
              },
              {
                startTime: 1632421800000,
                endTime: 1632508199999,
                dayEvents: [],
              },
              {
                startTime: 1632508200000,
                endTime: 1632594599999,
                dayEvents: [],
              },
            ],
            index: 4,
            sidebarScope: "Floor 5",
          },
          {
            eventList: [
              {
                startTime: 1631989800000,
                endTime: 1632076199999,
                dayEvents: [],
              },
              {
                startTime: 1632076200000,
                endTime: 1632162599999,
                dayEvents: [],
              },
              {
                startTime: 1632162600000,
                endTime: 1632248999999,
                dayEvents: [],
              },
              {
                startTime: 1632249000000,
                endTime: 1632335399999,
                dayEvents: [],
              },
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
            index: 5,
            sidebarScope: "Yem",
          },
        ],
      },
      eventPositions: {
        "Floor 1": {
          "Floor 1 - Event 3": 0,
          "Test 1": 0,
          "Floor 1 - Event 6": 0,
          "Event 1": 1,
          "Floor 1 - Event 5": 2,
          "Floor 1 - Event 4": 3,
          "Floor 1 - Event 2": 4,
        },
        "Floor 2": { "Event 2": 0, "Floor 2 - Event 2": 0 },
        "Floor 3": {},
        "Floor 4": {},
        "Floor 5": {},
        Yem: { "Event 3": 0 },
      },
    }
  },
  methods: {
    calculateStyle(event, blockIndex, sidebarName) {
      let { start, end, name } = event || {}
      let topPosition = this.eventPositions[sidebarName][name]

      let { startTime, endTime } = this.weekDays[blockIndex]
      let startScope = startTime <= start && endTime >= start

      let styleObj = {}

      if (startScope) {
        let widthDiff = 1

        if (end > endTime) {
          let remainingDiff = end - endTime
          widthDiff += Math.ceil(remainingDiff / this.dayTime)
        }

        styleObj.width = `calc((((100% - 7px) / 7) * ${widthDiff}) - ${widthDiff -
          1 +
          4}px)`
        styleObj.left = `calc((${blockIndex} * ((100% - 7px) / 7)) + ${2 +
          blockIndex}px)`
        styleObj.top = `${topPosition * 2}em`
      } else {
        styleObj.top = `${topPosition * 2}em`
      }
      console.log("called calculate style")
      return styleObj
    },
    applyClass(
      eventItem
      // , rowEvent
    ) {
      // let {
      //   isResized,
      //   isDragging,
      //   draggingElement,
      //   // resizingElement
      // } = this
      // let { eventName, sideScope } = draggingElement || {}
      // let { eventItemName, sideScope: resizeSideScope } = resizingElement || {}
      let {
        startScope,
        // , name
      } = eventItem || {}
      // let { sidebarScope } = rowEvent || {}

      return [
        "event-class",
        !startScope && "transparent d-none",
        // isResized && "resize",
        // !startScope &&
        //   !isResized &&
        //   (!isDragging ||
        //     (isDragging && eventName === name && sideScope === sidebarScope)) &&
        // "d-none",
      ]
    },
  },
}
</script>
<style>
.grid-container {
  display: grid;
  overflow: auto;
  height: 300px;
}
.flex {
  display: flex;
  flex-direction: column;
}
.grid-row {
  height: 70px;
  display: flex;
  flex-grow: 1;
}
.grid-row-fixed {
  position: sticky;
  top: 0;
  z-index: 2;
  background: white;
  height: 30px;
}
.grid-item {
  border: 1px solid gray;
  min-width: 100px;
  flex-grow: 1;
  width: 100px;
}
.grid-item-sidebar {
  width: 200px;
  position: sticky;
  background: white;
  left: 0;
  flex-grow: 0;
  z-index: 1;
}
.pinned-grid {
  position: sticky !important;
  top: 30px;
  z-index: 1;
  background: white;
}
</style>
<style>
.d-flex {
  display: flex;
}
.flex-grow {
  flex-grow: 1;
}
.d-none {
  display: none;
}
.transparent {
  background: transparent !important;
  border: transparent !important;
  width: calc((((100% - 7px) / 7) * 1) - 4px);
}
.table-sidebar-header {
  min-width: 150px;
  width: 150px;
  border: 1px solid;
}
.table-header-grid {
  display: flex;
  justify-content: space-between;
  width: 100%;
  overflow: scroll;
}
.table-header-item {
  min-width: 100px;
  width: 100%;
  border: 1px solid;
  border-left: none;
}
.table-grid {
  height: 6em;
  width: 14.29%;
  border-right: 1px solid;
  border-bottom: 1px solid;
}
.table-sidebar-content {
  border-left: 1px solid;
  border-bottom: 1px solid;
  border-right: 1px solid;
  display: flex;
  width: 150px;
  min-width: 150px;
  height: 6em;
}
.sidebar-content-item {
  margin: auto;
  display: flex;
  align-items: center;
}
.table-grid:last-child {
  border-right: 1px solid;
}
.event-class {
  position: absolute;
  height: 20px;
  background: lavender;
  font-size: 12px;
  /* cursor: pointer; */
  cursor: grab;
  border: 1px solid mediumslateblue;
}
.my-handler {
  cursor: move;
}
.event-item-class {
  width: calc(100% - 20px);
  display: flex;
  justify-content: center;
  align-items: center;
}
.position-relative {
  position: relative;
}
.sidebar-class {
  position: sticky;
  z-index: 1;
  left: 0;
  background: antiquewhite;
}
.text-overflow-ellipsis {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.event-resize {
  display: flex;
  justify-content: space-between;
  height: 100%;
}
.resize-div {
  width: 10px;
  /* cursor: ew-resize; */
  cursor: col-resize;
}
.resize-div:hover {
  background: mediumslateblue;
}
.transparent .resize-div:hover {
  background: transparent;
}
.vue-recycle-scroller .resize-observer {
  height: 0px;
}
.vue-recycle-scroller
  .vue-recycle-scroller__item-wrapper
  .vue-recycle-scroller__item-view {
  transform: translateY(0px) !important;
}
img {
  height: 15px;
  width: 15px;
  padding-right: 5px;
  visibility: hidden;
}
.table-sidebar-content:hover img,
.table-sidebar-content img.pinned {
  visibility: visible;
}
.dragging {
  cursor: grabbing;
}
.resize {
  cursor: col-resize;
}
.resize-highlighter {
  background: paleturquoise;
}
.unschedule-class {
  height: 100%;
  padding-left: 5px;
  border: 1px solid grey;
  box-shadow: -5px 3px 4px 0 lightgrey;
}
.hide-unscheduled-event {
  cursor: pointer;
  color: gray;
  text-align: end;
  margin-bottom: 10px;
}
.unscheduled-event {
  background: lavender;
  font-size: 12px;
  border: 1px solid mediumslateblue;
  cursor: grab;
  height: 20px;
  margin: 0 5px 5px 0px;
}
/* .opacity_half {
  opacity: 0.6;
} */
</style>
