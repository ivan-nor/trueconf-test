<template>
    <div class="buttons">
      <button
        v-for="floor in floors"
        :key="floor.id"
        @click="callButton(floor)"
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
  margin: 10px 0px 10px 10px;
  cursor: pointer;
}
</style>
