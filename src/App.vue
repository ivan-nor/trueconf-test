<template>
  <div class="app">
    <input type="number" min="1" max="1" v-model="numberOfElevators" @change="changeElevators"/>
    <p>Число лифтов: {{ numberOfElevators }}</p>
    <input type="number" min="1" max="5" v-model="numberOfFloors" @change="changeFloors"/>
    <p>Число этажей: {{ numberOfFloors }}</p>
    <div class="shaft">
      <div v-for="elevator in elevators" :key="elevator.id">
        <ElevatorComponent
          :id="elevator.id"
          :currentFloor="elevator.currentFloor"
          :numberOfFloors="numberOfFloors"
        />
      </div>
      <p>{{ numberOfFloors }}</p>
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
import { uniqueId } from 'lodash'

export default {
  name: 'App',
  components: {
    ElevatorComponent,
    CallButtons
  },
  data () {
    const elevators = []
    for (let i = 1; i <= this.$data.numberOfElevators; i += 1) {
      elevators.push({ id: uniqueId() + 330, currentFloor: 1 })
    }
    const floors = []
    for (let j = 1; j < this.$data.numberOfFloors; j += 1) {
      floors.push({ id: uniqueId() })
      console.log('floors', floors)
    }
    return {
      numberOfElevators: 2,
      numberOfFloors: 5,
      elevators,
      floors
    }
  },
  updated () {
    console.log('updated APP')
  },
  mounted () {
    console.log('mounted APP', this.$data)
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
