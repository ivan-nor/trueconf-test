<template>
  <div class="elevator-shaft">
    <div class="elevator" :style="elevatorStyle">{{ elevator.id }} {{ elevator.currentFloor }}</div>
  </div>
</template>

<script>
export default {
  props: {
    elevator: Object,
    floors: Object
  },
  data () {
    return {
      // elevatorStyle: {
      //   'margin-bottom': '10px'
      // }
    }
  },
  watch: {
    // elevatorStyle: function (prev, next) {
    //   const prevValue = prev['margin-bottom'].match(/^\d+/)[0]
    //   const nextValue = next['margin-bottom'].match(/^\d+/)[0]
    //   console.log('WATCH', prev, next, Math.abs(prevValue - nextValue))
    // }
  },
  computed: {
    elevatorStyle () {
      const { currentFloor, prevFloor } = this.elevator
      return ({ 'margin-bottom': `${10 + (this.elevator.currentFloor - 1) * 50}px`, transition: `margin-bottom ${Math.abs(currentFloor - prevFloor)}s ease` })
    }
  },
  updated () {
    console.log('update ELEVATOR', this.elevator, this.elevator.currentFloor, this.elevatorStyle)
  },
  methods: {
    // moveElevator (newFloor, oldFloor) { // ДОЛЖНА БЫТЬ АНИМАЦИЯ ЗДЕСЬ ?
    //   console.log('move elevator', newFloor, oldFloor)
    // const floorHeight = 100

    // const positionChange = (oldFloor - newFloor) * floorHeight

    // const elevatorElement = this.$ref.querySelector('.elevator')
    // elevatorElement.style.transition = 'bottom 1s ease'
    // elevatorElement.style.bottom = `${positionChange}px`

    // setTimeout(() => {
    //   elevatorElement.style.transition = 'none'
    //   elevatorElement.style.bottom = '0px'
    // }, 500)
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
  justify-content: flex-end; /* Чтобы лифт был внизу */;
}

.elevator {
  width: 30px;
  height: 30px;
  background-color: #3498db;
  border: none;
  border-radius: 10%;
  margin: 10px 0;
  text-align: center;
  line-height: 30px;
}
</style>
