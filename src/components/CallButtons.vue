<template>
    <div class="buttons">
      <button
        v-for="floor in floors"
        :key="floor.id"
        @click="callButton(floor.id)"
      >
        {{ floor.id }}
      </button>
    </div>
</template>

<script>
export default {
  props: {
    floors: Array,
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
