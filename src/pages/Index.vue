<template>
  <q-page class="flex flex-start fit">
    <q-card class="full-width q-pa-xs">
      <q-list dense>
        <template v-for="device in devices" :key="device.host">
          <Device
            :ref="
              (el) => {
                if (el) refs[device.host] = el
              }
            "
            :device="device"
          />
        </template>
      </q-list>
    </q-card>
  </q-page>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue"
import { api } from "boot/axios"
import Device from "components/Device"

export default {
  name: "PageIndex",
  components: { Device },

  setup() {
    const refs = ref({})
    const devices = ref(() => [])
    const ws = new WebSocket("ws://localhost:3333/devices")
    const handler = (e) => {
      const { first: host, second: items } = JSON.parse(e.data)
      refs.value[host].appendItems(items)
    }

    ws.onmessage = handler

    onMounted(async () => {
      const { data } = await api.get("/devices")
      devices.value = data
    })

    onUnmounted(() => {
      ws.onmessage = undefined
      ws.close()
    })

    return { devices, refs }
  },
}
</script>
