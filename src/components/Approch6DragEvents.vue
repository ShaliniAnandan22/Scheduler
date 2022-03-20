<template>
  <div class="d-flex">
    <div class="d-flex flex-grow">
      <div class="sidebar-class">
        <div>
          <div class="table-sidebar-header">{{ sidebarHeader }}</div>
          <div>
            <div
              v-for="(sidebarItem, index) in sidebarListArray"
              :key="`sidebar-${index}`"
              class="table-sidebar-content"
              :style="getBlockHeight(sidebarItem.name)"
              @click="pinSideBarElement(sidebarItem)"
            >
              <div class="sidebar-content-item">
                <img
                  src="../assets/anchor.svg"
                  :class="{ pinned: sidebarItem.pinned }"
                />
                {{ sidebarItem.name }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="flex-grow">
        <div>
          <div class="table-header-grid">
            <div
              v-for="(day, dayIndex) in weekDays"
              :key="`header-${dayIndex}`"
              :class="[
                'table-header-item',
                resizeHighlighter.start <= dayIndex &&
                  resizeHighlighter.end >= dayIndex &&
                  'resize-highlighter',
              ]"
            >
              {{ day.name }}
            </div>
          </div>

          <div>
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
                          @click="eventClicked(eventItem)"
                          :draggable="!isResized && eventItem.startScope"
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
                        >
                          <!-- <div
                            class="text-overflow-ellipsis event-resize"
                            v-bind="
                              bindElementDetails(
                                eventIdx,
                                rowEvent.sidebarScope,
                                rowEvent.index,
                                eventItem.name
                              )
                            "
                          > -->
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
                              {{ eventItem.startScope ? eventItem.time : null }}
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
                              handleEvents(
                                $event,
                                'mousedown-resize',
                                eventItem
                              )
                            "
                          ></div>
                          <!-- </div> -->
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

    <div style="position: sticky;z-index: 1;right: 0px;background: white;">
      <div
        v-if="showUnScheduledEvents"
        class="unschedule-class"
        style="width: 150px;padding-right: 5px;"
      >
        <div
          @click="showUnScheduledEvents = false"
          class="hide-unscheduled-event"
        >
          Hide
        </div>
        <div style="display: flex;flex-wrap: wrap;">
          <div
            v-for="(event, idx) in unScheduledEvents"
            :key="`${event.name}_${idx}`"
            draggable="true"
            class="unscheduled-event"
            :event-name="event.name"
          >
            {{ event.name }}
          </div>
        </div>
      </div>
      <div
        v-else
        @click="showUnScheduledEvents = true"
        class="unschedule-class"
      >
        <img
          src="../assets/menu.svg"
          style="visibility: visible;cursor: pointer;"
        />
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
        { name: "Floor 3", pinned: false },
        { name: "Floor 4", pinned: false },
        { name: "Floor 5", pinned: false },
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
        "Floor 3": [],
        "Floor 4": [],
        "Floor 5": [],
      },
      eventList: [],
      eventPositions: {},
      quaterTime: 21600000,
      dayTime: 86400000,
      isResized: false,
      isDragging: false,
      resizingElement: null,
      draggingElement: null,
      sidePosition: null,
      resizeHighlighter: { start: -1, end: -1 },
      showUnScheduledEvents: false,
      unScheduledEvents: [
        { name: "Test-US-1" },
        { name: "Test-US-2" },
        { name: "Test-US-3" },
        { name: "Test-US-4" },
      ],
    }
  },
  created() {
    this.init()
    this.addDragEventListener()
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
    applyClass(eventItem, rowEvent) {
      let {
        isResized,
        isDragging,
        draggingElement,
        // resizingElement
      } = this
      let { eventName, sideScope } = draggingElement || {}
      // let { eventItemName, sideScope: resizeSideScope } = resizingElement || {}
      let { startScope, name } = eventItem || {}
      let { sidebarScope } = rowEvent || {}

      return [
        "event-class text-overflow-ellipsis d-flex",
        !startScope && "transparent",
        isResized && "resize",
        !startScope &&
          // (!isResized ||
          //   (isResized &&
          //     eventItemName === name &&
          //     resizeSideScope === sidebarScope))
          !isResized &&
          (!isDragging ||
            (isDragging && eventName === name && sideScope === sidebarScope)) &&
          "d-none",
      ]
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
      let { target } = event
      let { attributes, parentElement } = target || {}
      let blockIndex = parseInt(attributes["block-index"].value)
      let rowIndex = parseInt(attributes["row-index"].value)
      let sideScope = attributes["sidebarScope"].value
      let sidePosition = attributes["side-position"]?.value
      let eventItemName = attributes["event-name"]?.value
      let { parentElement: eventTarget } = parentElement || {}

      if (eventName === "mousedown-resize") {
        this.isResized = true
        this.resizingElement = {
          eventName,
          blockIndex,
          rowIndex,
          sideScope,
          sidePosition,
          eventItem,
          eventItemName,
          event: eventTarget,
        }
      }

      if (this.isResized) {
        if (
          eventName === "mousemove"
          // && target.className === "table-grid"
        ) {
          let {
            blockIndex: elementBlockIdx,
            eventItem: elementEventItem,
            sideScope: elementSideScope,
            sidePosition: elementPosition,
            event: elementEvent,
          } = this.resizingElement || {}
          let { start, end, name } = elementEventItem

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
            this.resizeHighlighter.start = blockIndex

            this.resizeHighlighter.end = this.weekDays.findIndex(
              (d) => d.startTime <= end && d.endTime >= end
            )
            // let { endTime } = this.weekDays[elementBlockIdx]

            // if (end > endTime) {
            //   let remainingDiff = end - endTime
            //   this.resizeHighlighter.end =
            //     elementBlockIdx + Math.ceil(remainingDiff / this.dayTime)
            // }

            let eventScopeListIdx = this.eventList.findIndex(
              (e) => e.sidebarScope === elementSideScope
            )
            let currentEventIdx = this.eventList[eventScopeListIdx].eventList[
              elementBlockIdx
            ].dayEvents.findIndex((e) => e.name === name)

            if (currentEventIdx >= 0) {
              this.eventList[eventScopeListIdx].eventList[
                elementBlockIdx
              ].dayEvents.splice(currentEventIdx, 1)
            }

            let blockEventIdx = this.eventList[eventScopeListIdx].eventList[
              blockIndex
            ].dayEvents.findIndex((e) => e.name === name)

            if (blockEventIdx >= 0) {
              this.eventList[eventScopeListIdx].eventList[
                blockIndex
              ].dayEvents.splice(blockEventIdx, 1, {
                ...elementEventItem,
                start,
                end,
              })
            } else {
              this.eventList[eventScopeListIdx].eventList[
                blockIndex
              ].dayEvents.push({
                ...elementEventItem,
                start,
                end,
              })
            }
          } else if (elementPosition === "end") {
            this.resizeHighlighter.start = elementBlockIdx
            this.resizeHighlighter.end = this.weekDays.findIndex(
              (d) => d.startTime <= end && d.endTime >= end
            )

            let elementEndBlockIdx = this.resizeHighlighter.end

            // let { endTime } = this.weekDays[elementBlockIdx]

            // if (end > endTime) {
            //   let remainingDiff = end - endTime
            //   elementBlockIdx += Math.ceil(remainingDiff / this.dayTime)
            // }
            // this.resizeHighlighter.end = elementBlockIdx

            if (elementEndBlockIdx < blockIndex) {
              end += (blockIndex - elementEndBlockIdx) * this.dayTime
            } else if (elementEndBlockIdx > blockIndex) {
              let movedEnd =
                end - (elementEndBlockIdx - blockIndex) * this.dayTime

              if (start < movedEnd) {
                end = movedEnd
              }
            }
            this.resizeHighlighter.end = blockIndex

            let eventScopeListIdx = this.eventList.findIndex(
              (e) => e.sidebarScope === elementSideScope
            )
            let currentEventIdx = this.eventList[eventScopeListIdx].eventList[
              elementBlockIdx
            ].dayEvents.findIndex((e) => e.name === name)

            if (currentEventIdx >= 0) {
              this.eventList[eventScopeListIdx].eventList[
                elementBlockIdx
              ].dayEvents.splice(currentEventIdx, 1, {
                ...elementEventItem,
                start,
                end,
              })
            }
          }

          elementEvent.style.zIndex = 1
          console.log(eventName, event, eventItem)
        }
      }

      if (["mouseup"].includes(eventName) && this.resizingElement) {
        // if (target.className === "table-grid") {
        let {
          blockIndex: elementBlockIdx,
          eventItem: elementEventItem,
          sideScope: elementSideScope,
          sidePosition: elementPosition,
          event: elementEvent,
        } = this.resizingElement || {}
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

        elementEvent.style.zIndex = ""
        console.log(
          eventName,
          blockIndex,
          rowIndex,
          sideScope,
          sidePosition,
          eventItem
        )
        // }

        this.isResized = false
        this.resizingElement = null
        this.resizeHighlighter = { start: -1, end: -1 }
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

      document.addEventListener(
        "dragstart",
        (event) => {
          // store a ref. on the dragged elem
          if (
            event.target.className.includes("event-class") ||
            event.target.className.includes("unscheduled-event")
          ) {
            dragged = event.target
            // make it half transparent
            event.target.style.cursor = "grabbing"
            event.target.style.opacity = 0.5
            event.target.classList.add("dragging")

            let { attributes } = dragged || {}
            let sideScope = attributes["sidebarScope"].value
            let eventItemName = attributes["event-name"]?.value

            this.draggingElement = { eventName: eventItemName, sideScope }
            this.isDragging = true
          }
        },
        false
      )

      document.addEventListener(
        "dragend",
        (event) => {
          // reset the transparency
          event.target.style.opacity = ""
          event.target.style.cursor = ""
          event.target.classList.remove("dragging")
          this.isDragging = false
          this.draggingElement = null
        },
        false
      )

      /* events fired on the drop targets */
      document.addEventListener(
        "dragover",
        (event) => {
          // prevent default to allow drop
          event.preventDefault()
        },
        false
      )

      document.addEventListener(
        "dragenter",
        (event) => {
          // highlight potential drop target when the draggable element enters it
          if (event.target.className === "table-grid") {
            event.target.style.background = "#f2fafc"
          }
        },
        false
      )

      document.addEventListener(
        "dragleave",
        (event) => {
          // reset background of potential drop target when the draggable element leaves it
          if (event.target.className === "table-grid") {
            event.target.style.background = ""
          }
        },
        false
      )

      document.addEventListener(
        "drop",
        (event) => {
          // prevent default action (open as link for some elements)
          event.preventDefault()
          // move dragged elem to the selected drop target
          // if (event.target.className === "table-grid") {
          event.target.style.background = ""

          dragged.style.opacity = ""
          dragged.style.cursor = ""
          dragged.classList.remove("dragging")

          let { attributes: blockAttributes } = event.target
          let blockIndex = parseInt(blockAttributes["block-index"].value)
          let sideScope = blockAttributes["sidebarScope"].value

          let { attributes: eventAttributes } = dragged
          let eventBlockIndex = parseInt(eventAttributes["block-index"]?.value)
          let eventSideScope = eventAttributes["sidebarScope"]?.value
          let eventItemName = eventAttributes["event-name"]?.value

          if (dragged.className === "unscheduled-event") {
            let eventIdx = this.unScheduledEvents.findIndex(
              (e) => e.name === eventItemName
            )
            let eventObj = this.unScheduledEvents[eventIdx]
            let { startTime, endTime } = this.weekDays[blockIndex]

            this.eventsArray[sideScope].push({
              ...eventObj,
              start: startTime,
              end: endTime,
            })
            this.unScheduledEvents.splice(eventIdx, 1)
          } else {
            let eventIdx = this.eventsArray[eventSideScope].findIndex(
              (e) => e.name === eventItemName
            )
            let eventObj = this.eventsArray[eventSideScope][eventIdx]
            let { start, end } = eventObj

            if (eventBlockIndex !== blockIndex) {
              if (eventBlockIndex > blockIndex) {
                let dayDiff = (eventBlockIndex - blockIndex) * this.dayTime
                eventObj.start = start - dayDiff
                eventObj.end = end - dayDiff
              } else {
                let dayDiff = (blockIndex - eventBlockIndex) * this.dayTime
                eventObj.start = start + dayDiff
                eventObj.end = end + dayDiff
              }
            }

            if (eventIdx >= 0) {
              if (eventSideScope !== sideScope) {
                this.eventsArray[eventSideScope].splice(eventIdx, 1)
                this.eventsArray[sideScope].push(eventObj)
              } else {
                this.eventsArray[eventSideScope].splice(eventIdx, 1, eventObj)
              }
            }
          }
          // }
          this.draggingElement = null
          this.isDragging = false
          this.init()
        },
        false
      )
    },
    eventClicked(eventItem) {
      console.log(`${eventItem.name} is clicked`)
    },
    blockClicked(sideScope, blockIdx) {
      console.log(`In ${sideScope}, ${blockIdx} is clicked`)
    },
  },
}
</script>
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
