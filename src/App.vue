<template>
  <div class="app">
    <input type="number" min="1" max="5" v-model="numberOfElevators"/>
    <p>Число лифтов: {{ numberOfElevators }}</p>
    <input type="number" min="2" max="5" v-model="numberOfFloors" />
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
// import _ from 'lodash'

export default {
  name: 'App',
  components: {
    ElevatorComponent,
    CallButtons
  },
  data () {
    return {
      numberOfElevators: 1,
      numberOfFloors: 5,
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
    // console.log('updated APP')
  },
  created () {
    this.setElevators()
    this.setFloors()
    // console.log('created')
  },
  computed: {
  },
  methods: {
    getNearestElevator (floor) { // floor: { id, currentElevator } ПРОДОЛЖИТЬ ЗДЕСЬ
      // const elevator = _.find(this.elevators, (o) => o.id === floor.currentElevator)
      console.log('ближайшее число к заданному', Math.abs(floor.id, this.elevators.map(e => e.currentFloor)))
      const elevator = {}
      console.log('get elevator:', floor.id, this.elevators, elevator)
      return elevator
    },

    callButton (floor) { // отсюда идет задача в стек вызовов лифта
      console.log('Вызван лифт на этаж:', floor, floor.id, floor.currentElevator) // floor: { id, currentElevator }
      const nearestElevator = this.getNearestElevator(floor)
      console.log('Ближайший лифт: ', nearestElevator)
      nearestElevator.currentFloor = floor.id
    },

    setElevators () {
      console.log('set elevators', this.numberOfElevators)
      const newElevators = []
      for (let i = 1; i <= this.numberOfElevators; i += 1) {
        newElevators.push({ id: i, currentFloor: 1 })
      }
      this.elevators = newElevators
    },

    setFloors () {
      console.log('set floors', this.numberOfFloors)
      const newFloors = []
      for (let i = 1; i <= this.numberOfFloors; i += 1) {
        newFloors.push({ id: i, currentElevator: null })
      }
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
