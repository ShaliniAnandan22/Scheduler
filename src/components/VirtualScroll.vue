<template>
  <RecycleScroller
    class="scroller"
    :items="list"
    :item-size="32"
    direction="horizontal"
    key-field="key"
    v-slot="{ item }"
  >
    <div class="user">
      {{ item }}
    </div>
  </RecycleScroller>
  <!-- direction="horizontal" -->
</template>

<script>
import { RecycleScroller } from "vue-virtual-scroller"
import "vue-virtual-scroller/dist/vue-virtual-scroller.css"

export default {
  components: { RecycleScroller },

  data() {
    return {
      list: Array(31)
        .fill()
        .map((_, i) => i + 1),
    }
  },
}
</script>
<style>
.scroller {
  height: 100%;
  width: 100%;
  min-height: 100px;
}

.user {
  /* height: 30px; */
  /* width: 30px; */
  padding: 0 12px;
  display: flex;
  align-items: center;
}
.vue-recycle-scroller.direction-horizontal .vue-recycle-scroller__item-wrapper {
  height: auto;
}
</style>
