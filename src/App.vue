<template>
  <div class="app">
    <input type="number" min="1" max="5" v-model="numberOfElevators"/>
    <p>Число лифтов: {{ numberOfElevators }}</p>
    <input type="number" min="2" max="10" v-model="numberOfFloors" />
    <p>Число этажей: {{ numberOfFloors }}</p>
    <div class="shaft">
      <div v-for="elevator in elevators" :key="elevator.id">
        <ElevatorComponent
          :floors="floors"
          :elevator="elevator"
          :status="elevator.status"
        />
      </div>
        <CallButtons
          :floors="floors"
          :elevators="elevators"
          @callButton="callButton"
          :queue="queue"
        />
    </div>
  </div>
</template>

<script>
import ElevatorComponent from './components/ElevatorComponent.vue'
import CallButtons from './components/CallButtons.vue'
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    ElevatorComponent,
    CallButtons
  },
  data () {
    return {
      numberOfElevators: localStorage.getItem('numberOfElevators') ?? 1,
      numberOfFloors: localStorage.getItem('numberOfFloors') ?? 5,
      elevators: [],
      floors: [],
      queue: new Set(),
      count: 0
    }
  },
  watch: {
    numberOfElevators () {
      this.setElevators()
    },
    numberOfFloors () {
      this.setFloors()
    }
  },
  methods: {
    watchCallsStack () {
      if (this.queue.size > 0) {
        this.callElevator(this.queue)
      }
      // setTimeout(this.watchCallsStack, 100) // ТАЙМЕР ОБНОВЛЕНИЯ
    },

    getNearestElevator (floorId) {
      const waitingElevators = this.elevators.filter(e => e.status === 'waiting') // свободные лифты
      const mappedOnModules = waitingElevators.map(e => ({ ...e, diff: Math.abs(e.currentFloor - floorId) })) // добавляем разницу с выбранным этажом
      const minDiff = Math.min(...mappedOnModules.map(e => e.diff)) // минимальная разница
      const minId = Math.min(...mappedOnModules.filter(e => e.diff === minDiff).map(e => e.id)) // минимальный ID среди лифтов с минимальной разницей
      const nearestElevatorId = mappedOnModules.reduce((acc, elevator) => {
        return (elevator.diff === minDiff && elevator.id === minId) ? elevator.id : acc
      }, {})

      return _.find(this.elevators, { id: nearestElevatorId })
    },

    callButton (floor) { // отсюда идет задача в стек вызовов лифта
      const findElevatorsOnFloor = this.elevators.filter((e) => e.currentFloor === floor.id) // лифты на этаже
      if (findElevatorsOnFloor.length === 0) {
        this.queue.add(Number(floor.id))
      }
      this.watchCallsStack()
    },

    callElevator () {
      console.log('CALL ELeVATOR', ++this.count, this.queue) // TODO: исправить вложенный таймаут
      this.queue.forEach((callingFloorId) => {
        const nearestElevator = this.getNearestElevator(callingFloorId)
        const currentElevators = this.elevators.filter(e => e.currentFloor === callingFloorId)
        console.log()

        if (!nearestElevator || currentElevators.length > 0) {
          return
        }

        nearestElevator.status = 'moving'
        nearestElevator.prevFloor = nearestElevator.currentFloor
        nearestElevator.currentFloor = callingFloorId

        const prevFloorOfNearestElevator = this.floors.filter(({ id }) => id === nearestElevator.prevFloor)[0]
        const currentFloorOfNearestElevator = this.floors.filter(({ id }) => id === nearestElevator.currentFloor)[0]
        prevFloorOfNearestElevator.elevatorsOnFloor = prevFloorOfNearestElevator.elevatorsOnFloor.filter(elevatorId => elevatorId !== nearestElevator.id)
        currentFloorOfNearestElevator.elevatorsOnFloor.push(nearestElevator.id)

        const time = Math.abs(nearestElevator.currentFloor - nearestElevator.prevFloor)

        setTimeout(() => {
          this.queue.delete(callingFloorId)
          nearestElevator.status = 'resting'

          setTimeout(() => {
            nearestElevator.status = 'waiting'
          }, 3000)
        }, time * 1000)
      })
    },

    setElevators () {
      localStorage.setItem('numberOfElevators', +this.numberOfElevators)
      const count = localStorage.getItem('numberOfElevators')
      const newElevators = []
      for (let i = 1; i <= count; i += 1) {
        newElevators.push({ id: i, currentFloor: 1, status: 'waiting', prevFloor: 1 }) // waiting, moving, resting
      }
      this.elevators = newElevators
    },

    setFloors () {
      localStorage.setItem('numberOfFloors', +this.numberOfFloors)
      const count = localStorage.getItem('numberOfFloors')
      const newFloors = []
      for (let i = 1; i <= count; i += 1) {
        newFloors.push({ id: i, elevatorsOnFloor: this.elevators.map(e => e.currentFloor).filter(c => c === i) })
      }
      localStorage.setItem('numberOfFloors', newFloors.length)
      this.floors = newFloors
    }
  },
  created () {
    this.setElevators()
    this.setFloors()
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.shaft {
  display: flex;
  border: 1px solid #ccc; // Граница вокруг шахт
  padding: 20px 10px 20px 20px; // Отступ вокруг шахт
}
</style>
