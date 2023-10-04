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
        />
      </div>
        <CallButtons
          :floors="floors"
          :elevators="elevators"
          @callButton="callButton"
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
      freeElevators: [],
      queue: []
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
  updated () {
    console.log('updated APP')
  },
  created () {
    this.setElevators()
    this.setFloors()
    console.log('created', localStorage.getItem('numberOfElevators'), localStorage.getItem('numberOfFloors'))
  },
  computed: {
  },
  methods: {
    getNearestElevator (floor) {
      const waitingElevators = this.elevators.filter(e => e.status === 'waiting') // свободные лифты
      const mappedOnModules = waitingElevators.map(e => ({ ...e, diff: Math.abs(e.currentFloor - floor.id) })) // добавляем разницу с выбранным этажом
      const nearestElevatorId = mappedOnModules.reduce((acc, elevator) => {
        const minDiff = Math.min(...mappedOnModules.map(e => e.diff)) // минимальная разница
        const minId = Math.min(...mappedOnModules.filter(e => e.diff === minDiff).map(e => e.id)) // минимальный ID среди лифтов с минимальной разницей
        return (elevator.diff === minDiff && elevator.id === minId) ? elevator.id : acc
      }, {})
      console.log('ближайший лифт:', _.find(this.elevators, { id: nearestElevatorId }))
      return _.find(this.elevators, { id: nearestElevatorId })
    },

    callButton (floor) { // отсюда идет задача в стек вызовов лифта
      console.log('Вызван лифт на этаж:', floor, floor.id) // floor: { id, currentElevator }
      const nearestElevator = this.getNearestElevator(floor)
      console.log('Ближайший лифт: ', nearestElevator)
      nearestElevator.prevFloor = nearestElevator.currentFloor
      nearestElevator.currentFloor = floor.id
    },

    callElevator (elevator, floor) {

    },

    setElevators () { // ДОБАВИТЬ ИЗМЕНЕНИЕ СЕССИОНСТОР
      localStorage.setItem('numberOfElevators', +this.numberOfElevators)
      const count = localStorage.getItem('numberOfElevators')
      console.log('set elevators', this.numberOfElevators, count)
      const newElevators = []
      for (let i = 1; i <= this.numberOfElevators; i += 1) {
        newElevators.push({ id: i, currentFloor: 1, status: 'waiting', prevFloor: null }) // waiting, moving, resting
      }
      this.elevators = newElevators
    },

    setFloors () { // ДОБАВИТЬ ИЗМЕНЕНИЕ СЕССИОНСТОР
      localStorage.setItem('numberOfFloors', +this.numberOfFloors)
      const count = localStorage.getItem('numberOfFloors')
      console.log('set floors', this.numberOfFloors, count)
      const newFloors = []
      for (let i = 1; i <= count; i += 1) {
        newFloors.push({ id: i })
      }
      localStorage.setItem('numberOfFloors', newFloors.length)
      this.floors = newFloors
    }
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
