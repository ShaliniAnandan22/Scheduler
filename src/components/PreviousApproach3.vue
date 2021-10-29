<template>
  <div>
    <!-- Header -->
    <div class="d-flex">
      <div class="table-sidebar-header">{{ sidebarHeader }}</div>
      <div class="table-header-grid">
        <div
          v-for="(day, dayIndex) in weekDays"
          :key="`header-${dayIndex}`"
          class="table-header-item"
        >
          {{ day.name }}
        </div>
      </div>
    </div>

    <!-- Event Data -->
    <div v-for="(sidebarItem, index) in sidebarList" :key="`sidebar-${index}`">
      <!-- Grids -->
      <div class="d-flex">
        <div class="table-sidebar-content">
          <div class="margin-auto">{{ sidebarItem }}</div>
        </div>
        <div class="table-header-grid">
          <div
            v-for="(day, dayIndex) in weekDays"
            :key="`grid-${dayIndex}`"
            class="table-grid"
          ></div>
        </div>
      </div>
      <!-- Events -->
      <div>
        <div style="position:relative">
          <div
            v-for="(event, eventIdx) in eventList[index]"
            :key="`day-event-${eventIdx}`"
          >
            <div v-if="event.dayEvents.length">
              <div
                v-for="(eventItem, eventItemIdx) in event.dayEvents"
                :key="eventItem.name"
                class="event-class"
                :style="calculateStyle(eventItem, eventItemIdx, eventIdx)"
              >
                <div>
                  <span>
                    <!-- Time -->
                    <span>{{ eventItem.time }}</span>
                    <!-- Event Name -->
                    <span>{{ eventItem.name }} </span>
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
