<template>
  <div class="app">
    <input type="number" min="1" max="5" v-model="numberOfElevators" @change="changeElevators"/>
    <p>Число лифтов: {{ numberOfElevators }}</p>
    <input type="number" min="1" max="5" v-model="numberOfFloors" @change="changeFloors"/>
    <p>Число этажей: {{ numberOfFloors }}</p>
    <div class="shaft">
      <div v-for="index in numberOfElevators" :key="index">
        <ElevatorComponent
          :id="i"
          :currentFloor="1"
          :numberOfFloors="numberOfFloors"
          :d="elevators.findIndex((o) => {
            console.log(o, elevators, elevators[index])
            return o.id === index
          })"
        />
      </div>
      <CallButtons
        :numberOfFloors="numberOfFloors"
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

let count = 100

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
      floors: []
    }
  },
  updated () {
    console.log('updated APP')
  },
  mounted () {
    const newElevators = []
    for (let i = 1; i <= this.$data.numberOfElevators; i += 1) {
      newElevators.push({ id: count++, currentFloor: 1 })
      this.$data.elevators = newElevators
    }
    const newFloors = []
    for (let j = 1; j < this.$data.numberOfFloors; j += 1) {
      newFloors.push({ id: j })
      // console.log('floors', floors)
    }
    this.$data.floors = newFloors
    console.log('mounted APP', this.$data)
  },
  computed: {
    getElevator (id) {
      return _.find(this.elevators, (o) => o.id === id)
    }
  },
  methods: {
    callButton (floor) { // отсюда идет задача в стек вызовов лифта
      console.log('Вызван лифт на этаж:', floor)
    },

    changeElevators (event) {
      console.log('change elevators')
    },
    changeFloors (event) {
      console.log('change floors')
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
  padding: 20px; // Отступ вокруг шахт
}
</style>
