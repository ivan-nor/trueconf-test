<template>
  <!-- <div class="call-buttons"> -->
      <!-- <div class="elevator" v-for="elevator in elevators" :key="elevator">
        {{ elevator }}
      </div> -->
    <!-- </div> -->
    <div class="buttons">
      <button
        v-for="floor in numberOfFloors"
        :key="floor"
        @click="callButton(floor)"
      >
        {{ floor }}
      </button>
    </div>
</template>

<script>
export default {
  props: {
    numberOfFloors: Number,
    elevators: Array
  },
  methods: {
    callButton (floor) {
      this.$emit('callButton', floor)
    }
  }
}
</script>

<style scoped>
.call-buttons {
  display: flex;
  align-items: flex-end; /* Выравнивание по нижнему краю */
}

.shaft {
  flex: 1; /* Растягиваем шахту на все доступное пространство */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.elevator {
  width: 30px;
  height: 30px;
  background-color: #3498db;
  border: none;
  border-radius: 50%;
  margin: 5px 0;
  text-align: center;
  line-height: 30px;
}

.buttons {
  display: flex;
  flex-direction: column-reverse; /* Отображение кнопок сверху вниз */
  align-items: center; /* Выравнивание по центру по горизонтали */
}

button {
  width: 30px;
  height: 30px;
  background-color: #e74c3c;
  border: none;
  border-radius: 50%;
  margin: 5px 0;
  cursor: pointer;
}
</style>
