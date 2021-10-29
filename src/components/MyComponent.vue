<template>
  <table>
    <thead>
      <tr>
        <td>
          <table>
            <thead>
              <tr>
                <th
                  rowspan="2"
                  style="min-width: 71px;
    height: 28px;
    border: 1px solid;"
                >
                  Sidebar
                </th>
                <th
                  v-for="date in weekDayDate"
                  :key="`header-${date}`"
                  class="table-class"
                >
                  {{ date }}
                </th>
              </tr>
              <tr>
                <th
                  v-for="date in weekDayDate"
                  :key="`header2-${date}`"
                  class="table-class"
                >
                  {{ `header2-${date}` }}
                </th>
              </tr>
            </thead>
          </table>
        </td>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>
          <div style="position: absolute;">
            <table>
              <tbody>
                <tr>
                  <!-- <th
                    v-for="(resourceTitle,
                    resourceTitleColIndex) in resourceData"
                    :key="`sidebar-${resourceTitleColIndex}`"
                  ></th> -->
                  <th
                    class="table-class heigth66"
                    style="min-width: 71px;"
                  ></th>
                  <td
                    v-for="dayDate in weekDayDate"
                    :key="`body-${dayDate}`"
                    class="table-class heigth66"
                    :rowspan="maxTotalEvent || 1"
                    :style="{
                      height: `calc(61px * ${maxTotalEvent || 1})`,
                      'padding-bottom': '7px',
                    }"
                  ></td>
                </tr>
              </tbody>
            </table>
          </div>
          <div>
            <table style="width: 100%;">
              <tbody>
                <tr
                  v-for="(eventData, rowIndex) in eventTableData"
                  :key="rowIndex"
                >
                  <!-- <th
                    v-for="(resourceTitle,
                    resourceTitleColIndex) in resourceData"
                    :key="resourceTitleColIndex"
                  >
                    {{ resourceTitle.name }}
                  </th> -->
                  <th :rowspan="eventData.data.length" style="min-width: 73px;">
                    {{ eventData.assetName }}
                  </th>
                  <td :colspan="31">
                    <!-- <tbody> -->
                    <tr
                      v-for="(dayEvent, index) in eventData.data"
                      :key="`${rowIndex}-${index}`"
                    >
                      <td
                        v-for="(evnt, evntIndex) in dayEvent"
                        :key="`${rowIndex}-${index}-${evntIndex}`"
                        :colspan="evnt.colspan || 1"
                        :class="[
                          'event-class',
                          evnt.name && 'event-class-data',
                        ]"
                        :style="{
                          width: `${48 * (evnt.colspan || 1)}px`,
                        }"
                      >
                        <template v-if="evnt.name">
                          {{ evnt.name }}
                        </template>
                      </td>
                    </tr>
                    <!-- </tbody> -->
                  </td>

                  <!-- <td
                    v-for="(dayEvent, index) in eventData.data"
                    :key="`${rowIndex}-${index}`"
                    :colspan="dayEvent.colspan || 1"
                    :class="[
                      'event-class',
                      dayEvent.name && 'event-class-data',
                    ]"
                    :style="{ width: `calc(48px * ${dayEvent.colspan || 1})` }"
                  >
                    <template v-if="dayEvent.name">
                      {{ dayEvent.name }}
                    </template>
                  </td> -->
                </tr>
              </tbody>
            </table>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
// import { RecycleScroller } from "vue-virtual-scroller"

export default {
  // components: { RecycleScroller },

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
      weekDayDate: Array(31)
        .fill()
        .map((_, i) => i + 1),
      events: [
        { name: "Test-1", start: 5, end: 7 },
        { name: "First Event", start: 12, end: 13 },
        { name: "Second Event", start: 13, end: 16 },
      ],
      eventTableData: [],
      monthEvents: [],
      resourceData: [
        { rowSpan: 1, name: "AC - 2", count: 1 },
        { rowSpan: 1, name: "TestAsset", count: 296 },
      ],
      // resourceTitles: [
      //   {
      //     resourceGroup: "HVAC",
      //     data: [{ rowSpan: 1, name: "AC - 2", count: 1 }],
      //     editable: true,
      //     id: 1070310,
      //   },
      //   {
      //     resourceGroup: "HVAC",
      //     data: [{ rowSpan: 1, name: "TestAsset", count: 296 }],
      //     editable: true,
      //     id: 1351546,
      //   },
      // ],
    }
  },

  created() {
    this.getMonthEvents()
    // this.setEventTableData()
  },

  computed: {
    maxTotalEvent() {
      let { monthEvents } = this
      return monthEvents.reduce((maxEvent, event) => {
        let { totalEvents } = event
        maxEvent = maxEvent < totalEvents ? totalEvents : maxEvent
        return maxEvent
      }, 0)
    },
  },

  watch: {
    maxTotalEvent() {
      this.setEventTableData()
    },
  },

  methods: {
    getMonthEvents() {
      this.weekDayDate.forEach((day) => {
        let dayEvents = this.events.filter((event) => {
          let { start, end } = event || {}
          return day >= start && day <= end
        })

        this.monthEvents.push({ dayEvents, totalEvents: dayEvents.length })
      })
    },
    setEventTableData() {
      let eventTableData = []
      Array(this.maxTotalEvent)
        .fill()
        .map((_, i) => i)
        .forEach((row) => {
          let eventdata = []
          let endDate = 0

          this.monthEvents.forEach((event, index) => {
            let { totalEvents, dayEvents } = event
            if (index + 1 <= endDate) return

            if (totalEvents && row < totalEvents) {
              let { name, start, end } = dayEvents[row]
              let canSkipRow = index + 1 > start && index + 1 <= end

              if (!canSkipRow) {
                eventdata.push({ name, colspan: end - start + 1 })
                endDate = end
              } else {
                eventdata.push({})
              }
            } else {
              eventdata.push({})
            }
          })

          eventTableData.push(eventdata)
        })
      this.eventTableData = [{ assetName: "TestAsset", data: eventTableData }]
    },
  },
}
</script>
<style scoped>
.table-class {
  min-width: 48px;
  height: 28px;
  border: 1px solid;
}
.heigth66 {
  height: 66px;
}
.event-class {
  height: 58px;
  min-width: 40px;
  border: 1px solid transparent;
}
.event-class-data {
  border: 1px solid blue;
  background-color: aliceblue;
  color: blue;
}
</style>
