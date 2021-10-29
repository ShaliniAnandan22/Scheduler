<template>
  <table>
    <thead>
      <tr>
        <th rowspan="2">
          Side-Header
        </th>
        <th
          v-for="(group, groupIndex) in groupColumns"
          :colspan="group.gridColumns.length"
          :key="groupIndex"
        >
          <div>{{ group.label }}</div>
        </th>
      </tr>
      <tr>
        <th v-for="column in gridColumns" :key="column.start">
          <div>
            {{ column.label }}
          </div>
        </th>
      </tr>
    </thead>

    <tbody>
      <template v-for="row in visibleRows">
        <tr v-if="row.rowIndex === 0" :key="'header-' + row.rowIndex">
          <th colspan="1">
            HVAC
          </th>
          <td :colspan="gridColumns.length"></td>
        </tr>

        <tr v-if="row.visibility" :key="row.rowIndex">
          <th
            v-for="(resourceTitle, resourceTitleColIndex) in resourceTitles[
              row.rowIndex
            ].data"
            :key="resourceTitleColIndex"
            :rowspan="resourceTitle.rowSpan"
          >
            {{ resourceTitle.name }}
          </th>
          <td
            v-for="(col, colIndex) in gridColumns"
            :key="row.data[colIndex].cellID"
            :colspan="row.data[colIndex].scheduledEnd ? 3 : 1"
          >
            <tbody>
              <tr>
                <td
                  v-for="(task, index) in row.data[colIndex].tasks"
                  :key="index"
                  class="record-card"
                  :colspan="task.scheduledEnd ? 3 : 1"
                >
                  {{ task["subject"] }}
                </td>
              </tr>
            </tbody>
            <!-- <div>
              <div
                v-for="(task, index) in row.data[colIndex].tasks"
                :key="index"
                class="record-card"
              >
                {{ task["subject"] }}
              </div>
            </div> -->
          </td>
        </tr>
      </template>
    </tbody>
  </table>
</template>
<script>
// import { API } from "@facilio/api"
import { groupColumns, gridColumns, visibleRows } from "../assets/dataUtil"

export default {
  data() {
    return {
      groupColumns: groupColumns,
      gridColumns: gridColumns,
      visibleRows: visibleRows,
      resourceTitles: [
        {
          resourceGroup: "HVAC",
          data: [{ rowSpan: 1, name: "AC - 2", count: 1 }],
          editable: true,
          id: 1070310,
        },
        {
          resourceGroup: "HVAC",
          data: [{ rowSpan: 1, name: "TestAsset", count: 296 }],
          editable: true,
          id: 1351546,
        },
      ],
    }
  },
  methods: {
    // async loadPlannerSettings() {
    //   let url = "v2/pmplanner/settings"
    //   let PlannerTypes = {
    //     ASSET_PLANNER: 1,
    //     STAFF_PLANNER: 2,
    //     SPACE_PLANNER: 3,
    //   }
    //   let params = { settings: { plannerType: PlannerTypes[this.plannerType] } }
    //   let { error, data } = await API.get(url, params)
    //   if (!error) {
    //     this.setting = data.settings
    //   }
    // },
  },
}
</script>
<style lang="css" scoped>
table {
  border: 1px solid;
}
th {
  border: 1px solid;
  min-width: 56px;
  line-height: 38px;
}
td {
  border: 1px solid burlywood;
}
.record-card {
  height: 19px;
  border: 1px solid rgba(255, 214, 72, 0.75);
  background: rgba(255, 214, 72, 0.2);
  font-size: 11px;
  font-weight: 500;
  line-height: 20px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  width: 54px;
  margin: 3px 0px;
}
</style>
