<template>
      <button
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
    queue: Set
  },
  methods: {
    callButton (floor) {
      this.$emit('callButton', floor)
    }
  },
  computed: {
    getBackgroung () {
      const color = this.queue.has(this.floor.id) ? 'yellow' : 'green'
      return { 'background-color': color }
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
