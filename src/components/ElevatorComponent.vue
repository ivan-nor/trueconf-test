<template>
  <div class="elevator-shaft">
    <div
      class="elevator"
      :style="elevatorStyle"
      :class="blinkingClass"
    >
      {{ getDirectionSymbol }} {{ elevator.currentFloor }}
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
  computed: {
    blinkingClass () {
      return this.status === 'resting' ? 'blinking' : ''
    },
    elevatorStyle () {
      const { currentFloor, prevFloor } = this.elevator
      const time = Math.abs(currentFloor - prevFloor)

      const marginBottom = `${10 + (currentFloor - 1) * 50}px`
      const transition = `margin-bottom ${time}s ease-in-out`

      return { marginBottom, transition }
    },
    getDirectionSymbol () {
      const { currentFloor, prevFloor } = this.elevator
      if (this.status !== 'moving') return null

      const direction = Math.sign(currentFloor - prevFloor)
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
</style>
