<template>
    <div class="buttons">
      <ButtonComponent
        v-for="floor in floors"
        :key="floor.id"
        @callButton="callButton"
        :floor="floor"
        :queue="queue"
      />
    </div>
</template>

<script>
import ButtonComponent from './ButtonComponent.vue'

export default {
  components: {
    ButtonComponent
  },
  props: {
    floors: Array,
    elevators: Array,
    queue: Set
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
</style>
