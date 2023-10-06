<template>
  <div class="elevator-shaft">
    <div
      class="elevator"
      :style="elevatorStyle"
      :class="blinkingClass"
      :direction="getDirection"
    >
      {{ getDirection }} {{ elevator.currentFloor }} {{ this.elevator.status }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    elevator: Object,
    floors: Object,
    status: String
  },
  data () {
    return {
      isBlinking: this.elevator.status === 'resting',
      direction: Math.sign(this.elevator.currentFloor - this.elevator.prevFloor),
      isMoving: this.elevator.status === 'resting'
    }
  },
  computed: {
    blinkingClass () {
      return this.status === 'resting' ? 'blinking' : ''
    },
    elevatorStyle () {
      const { currentFloor, prevFloor } = this.elevator
      const time = Math.abs(currentFloor - prevFloor)
      const transition = `margin-bottom ${time}s ease-in-out`
      const styles = { 'margin-bottom': `${10 + (currentFloor - 1) * 50}px`, transition }

      return styles
    },
    getDirection () {
      if (this.status !== 'moving') return null
      const direction = Math.sign(this.elevator.currentFloor - this.elevator.prevFloor)
      if (direction === 0) return null
      return (direction > 0) ? '↑' : '↓'
    }
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
  animation: blink 3s ease-in-out 0s infinite alternate forwards;
}

@keyframes blink {
  0% {
    background-color: #0fa;
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
