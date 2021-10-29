<template>
  <table>
    <thead>
      <tr>
        <td>
          <table>
            <tbody>
              <tr>
                <td>
                  <div>Events Group</div>
                </td>
              </tr>
            </tbody>
          </table>
        </td>
        <td class="table-mid-border"></td>
        <td>
          <table>
            <tbody>
              <tr>
                <td v-for="day in weekDays" :key="day">{{ day }}</td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>
          <table>
            <tbody>
              <tr
                v-for="(sideBarItem, index) in sideBarList"
                :key="`sidebar-${index}`"
              >
                <td>
                  <div>{{ sideBarItem }}</div>
                </td>
              </tr>
            </tbody>
          </table>
        </td>
        <td class="table-mid-border"></td>
        <td>
          <div>
            <!-- Events handling -->
            <div style="position: absolute;">
              <table>
                <tbody>
                  <tr
                    v-for="(eventItem, index) in eventsList"
                    :key="`event-list-${index}`"
                  >
                    <td>
                      <!-- events with calculated postion -->
                      <div style="width:100%;">
                        <!-- <div v-for="(day, index) in weekDays" :key="day">
                          <div
                            v-for="quater in [0, 1, 2, 3]"
                            :key="quater"
                            class="block-division"
                            :style="{
                              transform: `translateX(${quater *
                                25 *
                                (index + 1)}px)`,
                            }"
                          ></div>
                          <div
                            :style="{
                              transform: `translateX(${100 * (index + 1) +
                                2 * (index + 1)}px)`,
                            }"
                            class="block-division-border"
                          ></div>
                        </div> -->
                        <div
                          v-for="(event, eventIdx) in eventItem"
                          :key="`event=${eventIdx}`"
                          :style="calculateStyle(event)"
                          @dragover="handleEvents($event, 'dragover', event)"
                          @drop="handleEvents($event, 'drop', event)"
                          @dragenter="handleEvents($event, 'dragenter', event)"
                          @dragleave="handleEvents($event, 'dragleave', event)"
                          @dragstart="handleEvents($event, 'dragstart', event)"
                          @dragend="handleEvents($event, 'dragend', event)"
                          draggable="true"
                        >
                          {{ event.name }}
                        </div>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- Grid handling -->
            <div>
              <table>
                <tbody>
                  <tr
                    v-for="(sideBarItem, index) in sideBarList"
                    :key="`grid-${index}`"
                  >
                    <td v-for="day in weekDays" :key="day"></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  data() {
    return {
      weekDays: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
      sideBarList: ["Floor 1", "Floor 2", "Yem"],
      eventsList: [
        [{ name: "Event 1", start: 1631514600000, end: 1631709000000 }],
        [{ name: "Event 2", start: 1631406600000, end: 1631514600000 }],
        [{ name: "Event 3", start: 1631795400000, end: 1631989799999 }],
      ],
      startTime: 1631385000000,
      endTime: 1631989799999,
    }
  },
  methods: {
    calculateStyle(event) {
      let quaterDay = 21600000
      let eachBlockWidth = 26
      let { start, end } = event || {}
      let timeDifference = start - this.startTime
      let totalBlocks = timeDifference / quaterDay
      let totalMargin = totalBlocks * eachBlockWidth + (totalBlocks / 4) * 2
      let totalWidth = ((end - start) / quaterDay) * eachBlockWidth

      return {
        "margin-left": `${totalMargin}px`,
        width: `${totalWidth}px`,
        background: "yellowgreen",
        height: "20px",
        cursor: "pointer",
      }
    },
    handleEvents(event, eventName, eventObj) {
      console.log(eventName, eventObj, event)
    },
  },
}
</script>
<style scoped>
.table-mid-border {
  min-width: 3px;
  border-style: double;
  background: #eee;
  border-color: #ddd;
}
td {
  min-width: 100px;
  height: 50px;
  border: 1px solid;
}
.block-division {
  width: 26px;
  border-right: 1px solid chocolate;
  height: 50px;
  position: absolute;
}
.block-division-border {
  width: 2px;
  height: 50px;
  position: absolute;
}
/* table {
  width: 100%;
} */
</style>
