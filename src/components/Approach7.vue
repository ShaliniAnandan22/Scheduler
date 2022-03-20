<template>
  <div>
    <!-- <div class="d-flex">
      <div class="table-sidebar-header sticky-header">Space</div>
      <div class="table-header-grid">
        <div
          v-for="dayName in weekDays"
          :key="dayName.name"
          class="table-header-item"
        >
          {{ dayName.name }}
        </div>
      </div>
    </div> -->
    <div class="d-flex">
      <div class="sticky-header">
        <div class="table-sidebar-header">Space</div>
        <div class="table-height">
          <div
            v-for="(sidebarItem, index) in sidebarList"
            :key="`sidebar-${index}`"
            class="table-sidebar-content"
          >
            <!-- <img src="../assets/anchor.svg" /> -->
            {{ sidebarItem.name }}
          </div>
        </div>
      </div>
      <div class="flex-grow">
        <div class="table-header-grid">
          <div
            v-for="dayName in weekDays"
            :key="dayName.name"
            class="table-header-item"
          >
            {{ dayName.name }}
          </div>
        </div>
        <div class="table-height">
          <div v-for="(rowEventDetails, name) in eventListArray" :key="name">
            <template v-for="rowEvent in rowEventDetails">
              <div
                :key="rowEvent.index"
                v-if="rowEvent"
                class="position-relative"
              >
                <div
                  :index="rowEvent.index"
                  class="position-relative table-header-grid"
                >
                  <div
                    v-for="(event, eventIdx) in rowEvent.eventList"
                    :key="`day-event-${eventIdx}`"
                    class="table-grid"
                  ></div>
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
export default {
  data() {
    return {
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
    }
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

.sticky-header {
  position: sticky;
  z-index: 1;
  left: 0;
  background: white;
}
.table-height {
  height: calc(100vh - 30px);
}
</style>
