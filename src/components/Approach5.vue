<template>
  <div>
    <div class="d-flex">
      <div class="sidebar-class">
        <div>
          <div class="table-sidebar-header">{{ sidebarHeader }}</div>
          <div
            v-for="(sidebarItem, index) in sidebarListArray"
            :key="`sidebar-${index}`"
            class="table-sidebar-content"
            :style="getBlockHeight(sidebarItem.name)"
            @click="pinSideBarElement(sidebarItem)"
          >
            <div class="sidebar-content-item">
              <img
                src="../../public/anchor.svg"
                :class="{ pinned: sidebarItem.pinned }"
              />
              {{ sidebarItem.name }}
            </div>
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

          <div v-for="(rowEventDetails, name) in eventListArray" :key="name">
            <!-- presentation -->
            <template v-for="rowEvent in rowEventDetails">
              <div
                :key="rowEvent.index"
                v-if="rowEvent"
                class="position-relative"
              >
                <div
                  :index="rowEvent.index"
                  class="position-relative table-header-grid"
                  @mousemove.stop="handleEvents($event, 'mousemove', {})"
                  @mouseup.stop="handleEvents($event, 'mouseup', {})"
                >
                  <div
                    v-for="(event, eventIdx) in rowEvent.eventList"
                    :key="`day-event-${eventIdx}`"
                    class="table-grid"
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
                        :class="[
                          'event-class',
                          !eventItem.startScope && 'd-none',
                          isDragged &&
                            eventItem.name ===
                              elementDetailsToListen.eventItem.name &&
                            'opacity_half',
                        ]"
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
                        @mousedown="
                          handleEvents($event, 'mousedown', eventItem)
                        "
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
                              handleEvents(
                                $event,
                                'mousedown-resize',
                                eventItem
                              )
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
                            <span
                              style="padding-right: 5px;"
                              v-bind="
                                bindElementDetails(
                                  eventIdx,
                                  rowEvent.sidebarScope,
                                  rowEvent.index,
                                  eventItem.name
                                )
                              "
                            >
                              {{ eventItem.time }}
                            </span>
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
                              {{ eventItem.name }}
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
                              handleEvents(
                                $event,
                                'mousedown-resize',
                                eventItem
                              )
                            "
                          ></div>
                        </div>
                      </div>
                    </template>
                  </div>
                </div>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import sortBy from "lodash/sortBy"

export default {
  data() {
    return {
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
      sidebarList: [
        { name: "Floor 1", pinned: false },
        { name: "Floor 2", pinned: false },
        { name: "Yem", pinned: false },
      ],
      eventsArray: {
        "Floor 1": [
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
        "Floor 2": [
          { name: "Event 2", start: 1632011400000, end: 1632119400000 },
          {
            name: "Floor 2 - Event 2",
            start: 1632200400000,
            end: 1632207600000,
          },
        ],
        Yem: [{ name: "Event 3", start: 1632400200000, end: 1632594599999 }],
      },
      eventList: [],
      eventPositions: {},
      quaterTime: 21600000,
      dayTime: 86400000,
      isDragged: false,
      isResized: false,
      elementDetailsToListen: null,
      sidePosition: null,
    }
  },
  created() {
    this.init()
  },
  computed: {
    sidebarListArray() {
      let sidebarList = [...this.sidebarList]
      let idx = sidebarList.findIndex((l) => l.pinned)
      let pinnedElement = null

      if (idx >= 0) {
        pinnedElement = sidebarList[idx]
        sidebarList.splice(idx, 1)
        sidebarList.unshift(pinnedElement)
      } else {
        sidebarList = sortBy(sidebarList, ["position"])
      }
      return sidebarList
    },
    eventListArray() {
      let { eventList, sidebarListArray } = this
      let isPinned = sidebarListArray[0].pinned
      let pinnedRow = []
      let sidebarOrder = sidebarListArray.map((l) => l.name)
      let eventIdx = 0
      let rowEventList = []

      sidebarOrder.forEach((o, index) => {
        let idx = eventList.findIndex((e) => e.sidebarScope === o)

        if ((isPinned && index !== 0) || !isPinned) {
          eventList[idx].index = eventIdx
          rowEventList.push(eventList[idx])
          eventIdx++
        } else {
          eventList[idx].index = "pinned-row"
          pinnedRow = eventList[idx]
        }
      })

      return {
        pinnedRow: isPinned ? [pinnedRow] : null,
        eventList: sortBy(rowEventList, ["index"]),
      }
    },
  },
  methods: {
    init() {
      this.constructEventList()
      this.constructEventPositionList()

      this.sidebarList = this.sidebarList.map((l, i) => ({
        ...l,
        position: i + 1,
      }))
      // this.addDragEventListener()
    },
    constructEventList() {
      this.eventList = []

      this.sidebarList.forEach((sidebarElement, index) => {
        let { name } = sidebarElement || {}
        let eventList = []
        this.weekDays.forEach((day) => {
          let { startTime, endTime } = day
          let dayEvents = sortBy(this.eventsArray[name], ["start"])
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
        this.eventList.push({ eventList, index, sidebarScope: name })
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
      this.eventPositions = {}
      this.sidebarList.forEach((sidebarElement, index) => {
        let { name: sidebarName } = sidebarElement || {}
        let eventArray = sortBy(this.eventsArray[sidebarName], ["start"])
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
        this.eventPositions[sidebarName] = eventPositionObj
      })
    },
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

        styleObj.width = `calc(${widthDiff * 14.29}% - ${widthDiff + 3}px)`
        styleObj.left = `calc(${blockIndex * 14.29}% + 2px)`
        styleObj.top = `${topPosition * 2}em`
      }
      return styleObj
    },
    getBlockHeight(sidebarScope) {
      let index = this.eventList.findIndex(
        (e) => e.sidebarScope === sidebarScope
      )
      let maxTotalEvents = this.getMaxTotal(index)

      if (maxTotalEvents > 3) {
        return {
          height: `${maxTotalEvents * 2}em`,
        }
      }
    },
    handleEvents(event, eventName, eventItem) {
      let { toElement } = event
      let { attributes } = toElement || {}
      let blockIndex = parseInt(attributes["block-index"].value)
      let rowIndex = parseInt(attributes["row-index"].value)
      let sideScope = attributes["sidebarScope"].value
      let sidePosition = attributes["side-position"]?.value
      let eventItemName = attributes["event-name"]?.value

      if (eventName === "mousedown-resize") {
        this.isResized = true
        this.elementDetailsToListen = {
          eventName,
          blockIndex,
          rowIndex,
          sideScope,
          sidePosition,
          eventItem,
          eventItemName,
        }
      }

      if (eventName === "mousedown") {
        this.isDragged = true
        this.elementDetailsToListen = {
          eventName,
          blockIndex,
          rowIndex,
          sideScope,
          eventItem,
          eventItemName,
        }
      }

      if (this.isDragged || this.isResized) {
        console.log(eventName, event, eventItem)
      }

      if (["mouseup"].includes(eventName)) {
        let {
          blockIndex: elementBlockIdx,
          eventItem: elementEventItem,
          sideScope: elementSideScope,
          sidePosition: elementPosition,
        } = this.elementDetailsToListen
        let { start, end, name } = elementEventItem

        if (this.isResized) {
          if (elementPosition === "start") {
            if (elementBlockIdx > blockIndex) {
              start -= (elementBlockIdx - blockIndex) * this.dayTime
            } else if (elementBlockIdx < blockIndex) {
              let movedStart =
                start + (blockIndex - elementBlockIdx) * this.dayTime

              if (end > movedStart) {
                start = movedStart
              }
            }
          } else if (elementPosition === "end") {
            let { endTime } = this.weekDays[elementBlockIdx]

            if (end > endTime) {
              let remainingDiff = end - endTime
              elementBlockIdx += Math.ceil(remainingDiff / this.dayTime)
            }

            if (elementBlockIdx < blockIndex) {
              end += (blockIndex - elementBlockIdx) * this.dayTime
            } else if (elementBlockIdx > blockIndex) {
              let movedEnd = end - (elementBlockIdx - blockIndex) * this.dayTime

              if (start < movedEnd) {
                end = movedEnd
              }
            }
          }

          let currentEventIdx = this.eventsArray[elementSideScope].findIndex(
            (e) => e.name === name
          )

          if (currentEventIdx >= 0) {
            this.eventsArray[elementSideScope].splice(currentEventIdx, 1, {
              ...elementEventItem,
              start,
              end,
            })
          }
        }

        if (this.isDragged) {
          if (blockIndex !== elementBlockIdx) {
            if (elementBlockIdx > blockIndex) {
              let dayDiff = (elementBlockIdx - blockIndex) * this.dayTime
              start -= dayDiff
              end -= dayDiff
            } else {
              let dayDiff = (blockIndex - elementBlockIdx) * this.dayTime
              start += dayDiff
              end += dayDiff
            }
          }

          let currentEventIdx = this.eventsArray[elementSideScope].findIndex(
            (e) => e.name === name
          )

          if (currentEventIdx >= 0) {
            if (elementSideScope !== sideScope) {
              this.eventsArray[elementSideScope].splice(currentEventIdx, 1)
              this.eventsArray[sideScope].push({
                ...elementEventItem,
                start,
                end,
              })
            } else {
              this.eventsArray[elementSideScope].splice(currentEventIdx, 1, {
                ...elementEventItem,
                start,
                end,
              })
            }
          }
        }

        console.log(
          eventName,
          blockIndex,
          rowIndex,
          sideScope,
          sidePosition,
          eventItem
        )

        this.isDragged = false
        this.isResized = false
        this.elementDetailsToListen = null
        this.init()
      }
    },
    pinSideBarElement(item) {
      let { pinned, position } = item
      let idx = this.sidebarList.findIndex((l) => l.position === position)

      if (pinned) {
        this.sidebarList[idx].pinned = false
      } else {
        this.sidebarList = this.sidebarList.map((l) => ({
          ...l,
          pinned: false,
        }))
        this.sidebarList[idx].pinned = true
      }
    },
    bindElementDetails(
      blockIdx,
      sidebarScopeName,
      rowIdx,
      eventName,
      sidePosition
    ) {
      let bindDetails = {
        "block-index": blockIdx,
        sidebarScope: sidebarScopeName,
        "row-index": rowIdx,
      }

      if (eventName) {
        bindDetails["event-name"] = eventName
      }
      if (sidePosition) {
        bindDetails["side-position"] = sidePosition
      }
      return bindDetails
    },
    addDragEventListener() {
      let dragged
      // document.addEventListener("drag", function(event) {}, false)

      document.addEventListener(
        "dragstart",
        function(event) {
          // store a ref. on the dragged elem
          dragged = event.target
          // make it half transparent
          event.target.style.opacity = 0.5
        },
        false
      )

      document.addEventListener(
        "dragend",
        function(event) {
          // reset the transparency
          event.target.style.opacity = ""
        },
        false
      )

      /* events fired on the drop targets */
      document.addEventListener(
        "dragover",
        function(event) {
          // prevent default to allow drop
          event.preventDefault()
        },
        false
      )

      document.addEventListener(
        "dragenter",
        function(event) {
          // highlight potential drop target when the draggable element enters it
          if (event.target.className == "dropzone") {
            event.target.style.background = "purple"
          }
        },
        false
      )

      document.addEventListener(
        "dragleave",
        function(event) {
          // reset background of potential drop target when the draggable element leaves it
          if (event.target.className == "dropzone") {
            event.target.style.background = ""
          }
        },
        false
      )

      document.addEventListener(
        "drop",
        function(event) {
          // prevent default action (open as link for some elements)
          event.preventDefault()
          // move dragged elem to the selected drop target
          if (event.target.className == "dropzone") {
            event.target.style.background = ""
            dragged.parentNode.removeChild(dragged)
            event.target.appendChild(dragged)
          }
        },
        false
      )
    },
  },
}
</script>
<style>
.d-flex {
  display: flex;
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
  height: 5em;
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
.opacity_half {
  opacity: 0.6;
}
</style>
