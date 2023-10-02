<template>
  <div class="elevator-shaft">
    <div class="elevator" :style="elevatorStyle">{{ d }}</div>
  </div>
</template>

<script>
export default {
  props: {
    currentFloor: Number,
    numberOfFloors: Number,
    id: Number,
    d: Number
  },
  data () {
    return {
      elevatorStyle: {
        bottom: 0
      }
    }
  },
  watch: {
    currentFloor (newFloor, oldFloor) {
      this.moveElevator(newFloor, oldFloor)
    }
  },
  updated () {
    console.log('elev upd')
  },
  methods: {
    // moveElevator (newFloor, oldFloor) {
    //   console.log('move elevator')
    //   const floorHeight = 100

    //   const positionChange = (oldFloor - newFloor) * floorHeight

    //   const elevatorElement = this.$ref.querySelector('.elevator')
    //   elevatorElement.style.transition = 'bottom 1s ease'
    //   elevatorElement.style.bottom = `${positionChange}px`

    //   setTimeout(() => {
    //     elevatorElement.style.transition = 'none'
    //     elevatorElement.style.bottom = '0px'
    //   }, 500)
    // }
  }
}
</script>

<style scoped>
.elevator-shaft {
  position: relative;
  width: 50px;
  height: 100%;
  background-color: #f2f2f2;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end; /* Чтобы лифт был внизу */
}

.elevator {
  width: 30px;
  height: 30px;
  background-color: #3498db;
  border: none;
  border-radius: 50%;
  margin: 5px 0;
  text-align: center;
  line-height: 30px;
}
</style>
