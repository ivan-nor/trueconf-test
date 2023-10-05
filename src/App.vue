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
    },
    queue () {
      console.log('APP watch QUEUE', this.queue)
      this.callElevator(this.queue)
    },
    elevators () {
      console.log('watch ELEV')
    }
  },
  methods: {
    watchCallsStack () {
      console.log('WATCH CALLS STACK', this.queue)
      // const nearestElevator = this.getNearestElevator()
      // setTimeout(this.watchCallsStack, 1000) // ТАЙМЕР ОБНОВЛЕНИЯ
    },
    getNearestElevator (id) {
      const waitingElevators = this.elevators.filter(e => e.status === 'waiting') // свободные лифты
      const mappedOnModules = waitingElevators.map(e => ({ ...e, diff: Math.abs(e.currentFloor - id) })) // добавляем разницу с выбранным этажом
      const nearestElevatorId = mappedOnModules.reduce((acc, elevator) => {
        const minDiff = Math.min(...mappedOnModules.map(e => e.diff)) // минимальная разница
        const minId = Math.min(...mappedOnModules.filter(e => e.diff === minDiff).map(e => e.id)) // минимальный ID среди лифтов с минимальной разницей
        return (elevator.diff === minDiff && elevator.id === minId) ? elevator.id : acc
      }, {})
      console.log('ближайший лифт:', _.find(this.elevators, { id: nearestElevatorId }))
      return _.find(this.elevators, { id: nearestElevatorId })
    },

    callButton (floor) { // отсюда идет задача в стек вызовов лифта
      const findElevatorsOnFloor = this.elevators.filter((e) => e.currentFloor === floor.id) // лифты на этаже
      console.log('лифты на этаже', typeof floor.id, floor.id, findElevatorsOnFloor)
      if (findElevatorsOnFloor.length === 0) {
        this.queue = [...this.queue, Number(floor.id)]
      }

      // console.log('Вызван лифт на этаж:', floor, floor.id) // floor: { id, currentElevator }
      // const nearestElevator = this.getNearestElevator(floor)
      // console.log('Ближайший лифт: ', nearestElevator)
      // nearestElevator.prevFloor = nearestElevator.currentFloor
      // nearestElevator.currentFloor = floor.id
      // nearestElevator.status = 'moving'
      // const time = Math.abs(nearestElevator.prevFloor - nearestElevator.currentFloor) + 3
      // // console.log('Вызван лифт на этаж:', floor, floor.id, 'Ближайший лифт: ', nearestElevator)
      // console.log('before', nearestElevator.prevFloor, nearestElevator.currentFloor, time, nearestElevator.status)
      // setTimeout(() => {
      //   nearestElevator.status = 'waiting'
      //   console.log('after', nearestElevator.status)
      // }, time * 1000)
    },

    callElevator (id) {
      console.log('CALL ELEVATOR QUEUE', this.queue, id)
      console.log('Вызван лифт на этаж:', id, this.queue[0]) // floor: { id, currentElevator }
      const nearestElevator = this.getNearestElevator(id)
      console.log('Ближайший лифт: ', nearestElevator)
      nearestElevator.prevFloor = nearestElevator.currentFloor
      nearestElevator.currentFloor = this.queue[0]
      nearestElevator.status = 'moving'
      const time = Math.abs(nearestElevator.prevFloor - nearestElevator.currentFloor) + 3
      // console.log('Вызван лифт на этаж:', floor, floor.id, 'Ближайший лифт: ', nearestElevator)
      console.log('before', nearestElevator.prevFloor, nearestElevator.currentFloor, time, nearestElevator.status)
      setTimeout(() => {
        nearestElevator.status = 'waiting'
        console.log('after', nearestElevator.status)
      }, time * 1000)
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
  },
  updated () {
    console.log('updated APP')
  },
  created () {
    this.setElevators()
    this.setFloors()
    console.log('created', localStorage.getItem('numberOfElevators'), localStorage.getItem('numberOfFloors'))
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
