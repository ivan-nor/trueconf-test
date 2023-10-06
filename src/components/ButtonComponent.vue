<template>
      <button
        :key="floor.id"
        @click="callButton(floor)"
        :style="getBackgroung"
      >
        <div>
          {{ floor.id }}
        </div>
      </button>
</template>

<script>
export default {
  props: {
    floor: Object,
    elevators: Array,
    queue: Set,
    status: Boolean,
    elevatorsOnFloor: Array
  },
  methods: {
    callButton (floor) {
      this.$emit('callButton', floor)
    }
  },
  computed: {
    getBackgroung () {
      const color = this.queue.has(this.floor.id) ? 'yellow' : 'green'
      const style = { 'background-color': color }
      return style
    }
  }
}
</script>

<style scoped>
button {
  width: 30px;
  height: 30px;
  border: none;
  border-radius: 50%;
  margin: 10px 0px 10px 10px;
  cursor: pointer;
}
</style>
