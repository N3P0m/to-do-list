<template>
  <button class="form__btn" @click="$emit('validate')">
    <img
      class="form__btn-img"
      src="~@/assets/img/close-card.svg"
      alt="Кнопка добавить задачу"
    />
  </button>
</template>

<script>
export default {
  name: "addTaskBtn",
};
</script>
