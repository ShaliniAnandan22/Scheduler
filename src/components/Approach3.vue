<template>
  <div>
    <div class="d-flex">
      <div class="sidebar-class">
        <div>
          <div class="table-sidebar-header">{{ sidebarHeader }}</div>
          <div
            v-for="(sidebarItem, index) in sidebarList"
            :key="`sidebar-${index}`"
            class="table-sidebar-content"
            :style="getBlockHeight(index)"
          >
            <div class="margin-auto">{{ sidebarItem }}</div>
          </div>
        </div>
      </div>

      <div style="flex-grow: 1;">
        <div>
          <div class="table-header-grid">
            <div
              v-for="(day, dayIndex) in weekDays"
              :key="`header-${dayIndex}`"
              class="table-header-item"
            >
              {{ day.name }}
            </div>
          </div>

          <div>
            <!-- presentation -->
            <RecycleScroller
              :items="eventList"
              key-field="index"
              :item-size="1"
              v-slot="{ item, index }"
              class="position-relative"
            >
              <div
                :index="index"
                class="position-relative table-header-grid"
                @mousemove.stop="handleEvents($event, 'mousemove', {}, index)"
                @mouseup.stop="handleEvents($event, 'mouseup', {}, index)"
              >
                <draggable
                  v-for="(event, eventIdx) in item.eventList"
                  :key="`day-event-${eventIdx}`"
                  class="table-grid"
                  :list="event.dayEvents"
                  draggable=".event-class"
                  @end="(ent) => updateList(ent, eventIdx)"
                  v-bind="fieldSectionDragOptions"
                  :index="eventIdx"
                  group="block"
                  :style="getBlockHeight(index)"
                >
                  <template v-if="event.dayEvents.length">
                    <div
                      v-for="(eventItem, eventItemIdx) in event.dayEvents"
                      :key="`${eventItem.name}-${eventItemIdx}`"
                      :class="[
                        'event-class',
                        !eventItem.startScope && 'd-none',
                      ]"
                      :style="calculateStyle(eventItem, eventIdx, index)"
                      :index="eventItemIdx"
                    >
                      <div class="text-overflow-ellipsis event-resize">
                        <div class="resize-div"></div>
                        <div class="event-item-class">
                          <!-- Time -->
                          <span style="padding-right: 5px;">
                            {{ eventItem.time }}
                          </span>
                          <!-- Event Name -->
                          <span>{{ eventItem.name }} </span>
                        </div>
                        <div
                          class="resize-div"
                          @mousedown.stop="
                            handleEvents(
                              $event,
                              'mousedown',
                              eventItem,
                              eventIdx
                            )
                          "
                          @mouseover="
                            handleEvents(
                              $event,
                              'mouseover',
                              eventItem,
                              eventIdx
                            )
                          "
                          @mouseout="
                            handleEvents(
                              $event,
                              'mouseout',
                              eventItem,
                              eventIdx
                            )
                          "
                          @mouseenter="
                            handleEvents(
                              $event,
                              'mouseenter',
                              eventItem,
                              eventIdx
                            )
                          "
                          @mouseleave="
                            handleEvents(
                              $event,
                              'mouseleave',
                              eventItem,
                              eventIdx
                            )
                          "
                        ></div>
                      </div>
                    </div>
                  </template>
                </draggable>
              </div>
            </RecycleScroller>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import sortBy from "lodash/sortBy"
import draggable from "vuedraggable"
import { RecycleScroller } from "vue-virtual-scroller"

export default {
  components: {
    draggable,
    RecycleScroller,
  },
  data() {
    return {
      fieldSectionDragOptions: {
        animation: 150,
        easing: "cubic-bezier(1, 0, 0, 1)",
        group: "tasksection",
        sort: true,
      },
      sidebarHeader: "Space",
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
      sidebarList: ["Floor 1", "Floor 2", "Yem"],
      eventsArray: [
        [
          { name: "Event 1", start: 1632099600000, end: 1632313800000 },
          {
            name: "Floor 1 - Event 2",
            start: 1632212100000,
            end: 1632276000000,
          },
          {
            name: "Floor 1 - Event 3",
            start: 1632027600000,
            end: 1632094200000,
          },
          {
            name: "Floor 1 - Event 4",
            start: 1632159000000,
            end: 1632232800000,
          },
          {
            name: "Floor 1 - Event 5",
            start: 1632146400000,
            end: 1632236400000,
          },
          {
            name: "Floor 1 - Event 6",
            start: 1632297600000,
            end: 1632319200000,
          },
          {
            name: "Test 1",
            start: 1632182400000,
            end: 1632200400000,
          },
        ],
        [
          { name: "Event 2", start: 1632011400000, end: 1632119400000 },
          {
            name: "Floor 2 - Event 2",
            start: 1632200400000,
            end: 1632207600000,
          },
        ],
        [{ name: "Event 3", start: 1632400200000, end: 1632594599999 }],
      ],
      currentView: "Week",
      eventList: [],
      eventPositions: [],
      quaterTime: 21600000,
      dayTime: 86400000,
      isDragged: false,
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.constructEventList()
      this.constructEventPositionList()
    },
    constructEventList() {
      this.eventList = []

      this.sidebarList.forEach((_, index) => {
        let eventList = []
        this.weekDays.forEach((day) => {
          let { startTime, endTime } = day
          let dayEvents = sortBy(this.eventsArray[index], ["start"])
            .filter((event) => {
              let { start, end } = event || {}
              return (
                (startTime <= start && endTime >= start) ||
                (startTime <= end && endTime >= end) ||
                (start < startTime && end > endTime)
              )
            })
            .map((event) => {
              let { start } = event
              let { startTime, endTime } = day
              let startScope = startTime <= start && endTime >= start
              let eventObj = {
                ...event,
                time: new Date(start).toLocaleTimeString(),
                startScope,
              }

              return eventObj
            })

          eventList.push({ startTime, endTime, dayEvents })
        })
        this.eventList.push({ eventList, index })
      })
    },
    getMaxTotal(index) {
      return this.eventList[index].eventList.reduce(
        (maxTotal, dayEventList) => {
          let { dayEvents } = dayEventList
          maxTotal = maxTotal < dayEvents.length ? dayEvents.length : maxTotal
          return maxTotal
        },
        0
      )
    },
    constructEventPositionList() {
      this.eventPositions = []
      this.sidebarList.forEach((_, index) => {
        let eventArray = sortBy(this.eventsArray[index], ["start"])
        let totalLoop = this.getMaxTotal(index)
        let eventPositionObj = {}
        let position = 0
        let eventNameToRemove = []

        while (totalLoop > 0) {
          let endEventTime = 0

          eventNameToRemove = []

          eventArray.forEach((event) => {
            let { start, end, name } = event

            if (start > endEventTime) {
              let endDate = new Date(end).getDate()
              let { endTime } = this.weekDays.find((d) => d.date === endDate)

              eventPositionObj[name] = position
              endEventTime = endTime
              // endEventTime = end
              eventNameToRemove.push(name)
            }
          })
          position++
          totalLoop--

          eventNameToRemove.forEach((name) => {
            let idx = eventArray.findIndex((e) => e.name === name)
            eventArray.splice(idx, 1)
          })
        }
        this.eventPositions.push(eventPositionObj)
      })
    },
    calculateStyle(event, blockIndex, spaceEventIdx) {
      // let { start, end } = event || {}
      let { start, end, name } = event || {}
      let topPosition = this.eventPositions[spaceEventIdx][name]

      let { startTime, endTime } = this.weekDays[blockIndex]
      let startScope = startTime <= start && endTime >= start

      let styleObj = {}

      if (startScope) {
        // let leftdiff = (start - startTime) / this.quaterTime
        let widthDiff = 1

        if (end > endTime) {
          let remainingDiff = end - endTime
          widthDiff += Math.ceil(remainingDiff / this.dayTime)
        }

        // styleObj.width = `${widthDiff * 3.57}%`
        // styleObj.left = `${blockIndex * 14.29 + parseInt(leftdiff) * 3.57}%`
        // styleObj.top = `${topPosition * 1.875}em`
        styleObj.width = `calc(${widthDiff * 14.29}% - ${widthDiff + 3}px)`
        styleObj.left = `calc(${blockIndex * 14.29}% + 2px)`
        styleObj.top = `${topPosition * 2}em`
      }
      return styleObj
    },
    getBlockHeight(index) {
      let maxTotalEvents = this.getMaxTotal(index)

      if (maxTotalEvents > 3) {
        return {
          height: `${maxTotalEvents + 3}em`,
        }
      }
    },
    updateList(events, fromIdx) {
      let { to, from, item } = events || {}
      let { __vue__, parentElement: toParentElement } = to || {}
      let { $attrs } = __vue__ || {}
      let { index: toIdx } = $attrs

      let { _underlying_vm_: eventObj } = item || {}

      let { parentElement: fromParentElement } = from || {}
      let { attributes: fromParentAttributes } = fromParentElement || {}
      let fromParentIdx = parseInt(fromParentAttributes.index.value)

      let { attributes: toParentAttributes } = toParentElement || {}
      let toParentIdx = parseInt(toParentAttributes.index.value)

      let { name: eventName, start, end } = eventObj
      let eventIdx = this.eventsArray[fromParentIdx].findIndex(
        (event) => event.name === eventName
      )

      if (fromIdx !== toIdx) {
        if (fromIdx > toIdx) {
          let dayDiff = (fromIdx - toIdx) * this.dayTime
          eventObj.start = start - dayDiff
          eventObj.end = end - dayDiff
        } else {
          let dayDiff = (toIdx - fromIdx) * this.dayTime
          eventObj.start = start + dayDiff
          eventObj.end = end + dayDiff
        }
      }

      if (fromParentIdx !== toParentIdx) {
        this.eventsArray[fromParentIdx].splice(eventIdx, 1)
        this.eventsArray[toParentIdx].push(eventObj)
      } else {
        this.eventsArray[fromParentIdx].splice(eventIdx, 1, eventObj)
      }

      this.init()
    },
    handleEvents(
      event,
      eventName
      // eventItem, eventBlock
    ) {
      if (eventName === "mousedown") {
        this.isDragged = true
      }

      if (this.isDragged) {
        // console.log(eventName, event, eventItem, eventBlock)
      }

      if (["mouseup", "mouseleave", "mouseout"].includes(eventName)) {
        this.isDragged = false
      }
    },
  },
}
</script>
<style>
.d-flex {
  display: flex;
}
.margin-auto {
  margin: auto;
}
.d-none {
  display: none;
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
  height: 5em;
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
  height: 5em;
}
.table-grid:last-child {
  border-right: 1px solid;
}
.event-class {
  position: absolute;
  height: 20px;
  background: lavender;
  font-size: 12px;
  cursor: pointer;
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
  cursor: ew-resize;
}
.vue-recycle-scroller .resize-observer {
  height: 0px;
}
.vue-recycle-scroller
  .vue-recycle-scroller__item-wrapper
  .vue-recycle-scroller__item-view {
  transform: translateY(0px) !important;
}
</style>
