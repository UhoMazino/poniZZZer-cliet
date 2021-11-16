<template>
  <q-expansion-item switch-toggle-side dense>
    <template #header>
      <q-item-section>
        <span>
          {{ device.name }}@{{ device.host }} [{{ total }}/512]
          <q-icon
            :color="device.active ? 'green' : 'red'"
            name="circle"
            size="xs"
          >
            <q-tooltip anchor="center end" self="center start">
              {{ device.active ? "online" : "offline" }}
            </q-tooltip>
          </q-icon>
        </span>
      </q-item-section>
    </template>

    <q-list dense>
      <transition-group
        enter-active-class="animated fadeInRight"
        leave-active-class="animated fadeOutLeft"
        appear
      >
        <template v-for="(list, i) in ports" :key="i">
          <Port :list="list" :index="i" />
        </template>
      </transition-group>
      <!-- <q-item>
        <q-btn @click="appendItems()" icon="add" size="sm" color="red" round />
      </q-item> -->
    </q-list>
  </q-expansion-item>
</template>

<script>
import { ref, reactive, computed } from "vue"
import Onu from "components/Onu"
import Port from "components/Port"
const def = {
  "e0:67:b3:c1:18:c7": {
    root: 1,
    port: 6,
    position: 53,
  },
  "e0:67:b3:c4:ec:07": {
    root: 1,
    port: 6,
    position: 54,
  },
  "e0:67:b3:c4:ec:2f": {
    root: 1,
    port: 6,
    position: 55,
  },
  "e0:67:b3:e6:ba:96": {
    root: 1,
    port: 6,
    position: 56,
  },
  "e0:67:b3:8f:cf:7b": {
    root: 1,
    port: 6,
    position: 57,
  },
  "e0:67:b3:92:af:5b": {
    root: 1,
    port: 6,
    position: 58,
  },
  "e0:e8:e6:1a:d2:a2": {
    root: 1,
    port: 6,
    position: 59,
  },
  "e0:e8:e6:14:3d:4d": {
    root: 1,
    port: 6,
    position: 60,
  },
  "e0:e8:e6:14:76:e3": {
    root: 1,
    port: 6,
    position: 61,
  },
  "e0:e8:e6:1a:a5:20": {
    root: 1,
    port: 6,
    position: 62,
  },
}
export default {
  name: "Device",
  components: { Onu, Port },
  props: { device: Object },
  setup(props) {
    const ports = reactive({})
    const total = computed(() =>
      Object.values(ports).reduce((c, v) => c + v.length, 0),
    )
    const appendItems = (items = def) => {
      for (let [key, value] of Object.entries(items)) {
        const result = { mac: key, ...value }
        if (ports[value.port]) {
          ports[value.port].push(result)
        } else ports[value.port] = [result]
      }
    }
    return {
      device: props.device,
      ports,
      total,
      appendItems,
    }
  },
}
</script>
