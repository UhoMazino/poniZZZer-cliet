<template>
  <q-item-label>{{ mac }}@{{ root }}/{{ port }}/{{ position }}</q-item-label>
</template>

<script>
export default {
  name: "Onu",
  props: { data: Object },
  setup(props) {
    const { mac, root, port, position } = props.data
    return { mac, root, port, position }
  },
}
</script>
