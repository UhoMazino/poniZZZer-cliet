<template>
  <q-expansion-item switch-toggle-side dense>
    <template #header>
      <q-item-section> Port {{ index }} [{{ list.length }}/64] </q-item-section>
    </template>
    <q-list dense>
      <transition-group
        enter-active-class="animated fadeInRight"
        leave-active-class="animated fadeOutLeft"
        appear
      >
        <q-item v-for="onu in list" :key="onu.mac">
          <Onu :data="onu" />
        </q-item>
      </transition-group>
    </q-list>
  </q-expansion-item>
</template>

<script>
import Onu from "components/Onu"
export default {
  components: { Onu },
  name: "Port",
  props: { list: Array, index: String },
  setup({ list, index }) {
    return { list, index }
  },
}
</script>
