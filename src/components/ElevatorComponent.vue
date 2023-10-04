<template>
  <div class="elevator-shaft">
    <div
      class="elevator"
      :style="elevatorStyle"
      :class="blinkingClass"
    >{{ elevator.id }} {{ elevator.currentFloor }}</div>
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
      isBlinking: false
    }
  },
  watch: {
    elevatorStyle: function (prev, next) { // исправить задержку и добавить изменение состояния лифта
      console.log('WATCH', prev, next)

      const { currentFloor, prevFloor } = this.elevator
      const time = Math.abs(currentFloor - prevFloor)
      const delay = time + 3
      console.log('ANIMATION time, delay', time, delay)

      this.isBlinking = true
      // this.elevator.status = 'resting'

      setTimeout(() => {
        this.isBlinking = false
        // this.elevator.status = 'waiting'
      }, delay * 1000)
    }
  },
  computed: {
    blinkingClass () {
      return this.isBlinking ? 'blinking' : ''
    },
    elevatorStyle () {
      const { currentFloor, prevFloor } = this.elevator
      const time = Math.abs(currentFloor - prevFloor)
      const transition = `margin-bottom ${time}s ease-in-out`
      const styles = { 'margin-bottom': `${10 + (currentFloor - 1) * 50}px`, transition }
      console.log('COMPUTED', styles)
      return styles
    }
  },
  updated () {
    // console.log('update ELEVATOR', this.elevator.id, this.elevator.currentFloor)
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

.blinking {
  animation: blink 3s ease-in-out 2s infinite alternate forwards;
}

@keyframes blink {
  0% {
    background-color: #3498db;
  }
  25% {
    background-color: #fff;
  }
  50% {
    background-color: #0fa;
  }
  75% {
    background-color: #fff;
  }
  100% {
    background-color: #3498db;
  }
}

@keyframes tempX {
  0%, 18%, 22%, 25%, 53%, 57%, 100% {
    background-color:
    #fff,
    #fff,
    #fff,
    #0fa,
    #0fa,
    #0fa,
    #0fa,
    #0fa;
  }
  20%, 24%, 55% {
      background-color: none;
  }
}
</style>
