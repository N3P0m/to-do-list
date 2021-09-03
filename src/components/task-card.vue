<template>
  <div class="task-card">
    <span class="task-card__name">{{ taskCard.name }}</span>
    <button class="task-card__btn-cls" @click="removeTask(taskCard)">
      <img
        class="task-card__btn-cls-img"
        src="~@/assets/img/close-card.svg"
        alt="Кнопка удалить задачу"
        @click="$emit('remove', taskCard)"
      />
    </button>
    <div class="task-card__container">
      <p class="task-card__text">{{ taskCard.text }}</p>
      <div class="task-card__checkbox">
        <label class="label">
          <input
            class="checkbox visually-hidden"
            type="checkbox"
            @change="$emit('addDone', taskCard)"
            :checked="!taskCard.status"
            alt="Задача выполнена"
            aria-label="Задача выполнена"
          />
          <span class="checkbox__custom"></span>
        </label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskCard",

  props: {
    taskCard: {
      type: Object,
    },
  },
  data() {
    return {};
  },
};
</script>
