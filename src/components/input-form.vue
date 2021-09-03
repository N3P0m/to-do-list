<template>
  <form class="container" action="" @submit.prevent :taskInput="taskInput">
    <div class="container-name">
      <input
        class="form__name"
        type="text"
        :placeholder="nameHolder"
        v-model="taskInput.name"
        :class="{ textDanger: isActive }"
      />
    </div>

    <div class="form__container">
      <div class="container-text">
        <input
          class="form__task"
          type="text"
          :placeholder="textHolder"
          v-model="taskInput.text"
          :class="{ textDanger: isActive }"
        />
      </div>
      <add-task-btn @validate="validate" />
    </div>
  </form>
</template>

<script>
import { gsap } from "gsap";
import addTaskBtn from "./add-task-btn.vue";

export default {
  name: "InputForm",
  components: { addTaskBtn },

  data() {
    return {
      taskInput: {
        name: "",
        text: "",
      },
      nameHolder: "Название...",
      textHolder: "Задача...",
      isActive: false,
    };
  },
  methods: {
    validate() {
      if (this.taskInput.name == "" || this.taskInput.text == "") {
        this.nameHolder = "Введите название!";
        this.textHolder = "Введите задачу!";
        this.isActive = true;
        if (this.taskInput.name == "") {
          gsap.fromTo(
            ".container-name",
            0.1,
            { x: -3 },
            { x: 3, clearProps: "x", repeat: 3 }
          );
        }
        if (this.taskInput.text == "") {
          gsap.fromTo(
            ".container-text",
            0.1,
            { x: -3 },
            { x: 3, clearProps: "x", repeat: 3 }
          );
        }
      } else {
        const taskCard = {
          id: Date.now(),
          name: this.taskInput.name,
          text: this.taskInput.text,
          status: true,
        };
        this.$emit("createTask", taskCard);
        this.taskInput.name = "";
        this.taskInput.text = "";

        this.nameHolder = "Название...";
        this.textHolder = "Задача...";
        this.isActive = false;
      }
    },
  },
};
</script>
