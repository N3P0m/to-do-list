<template>
  <div class="to-do-list-body">
    <h3 class="ta-c">Список задач</h3>
    <input-form @createTask="createTask" />
    <div class="card-list">
      <span class="card-list__divider">Активные</span>
      <transition
        @before-enter="beforeEnterStatus"
        @enter="enterStatus"
        @leave="leaveStatus"
      >
        <span class="task-status ta-c" v-if="taskCards.length == 0">
          Активных задач нет
        </span>
      </transition>
      <ul class="task-list">
        <transition-group
          @before-enter="beforeEnterTask"
          @enter="enterTask"
          @leave="leaveTask"
        >
          <li v-for="taskCard in taskCards" :key="taskCard.id">
            <task-card
              :taskCard="taskCard"
              @addDone="addDone"
              @remove="removeTask"
            />
          </li>
        </transition-group>
      </ul>
      <span class="card-list__divider">Выполнено</span>
      <transition
        @before-enter="beforeEnterStatus"
        @enter="enterStatus"
        @leave="leaveStatus"
      >
        <span class="task-status ta-c" v-if="taskCardsDone.length == 0"
          >Выполненых задач нет</span
        >
      </transition>
      <ul class="task-list">
        <transition-group
          @before-enter="beforeEnterTask"
          @enter="enterTask"
          @leave="leaveTask"
        >
          <li v-for="taskCard in taskCardsDone" :key="taskCard.id">
            <task-card
              :taskCard="taskCard"
              @addDone="addDone"
              @remove="removeTask"
              :class="{ 'task-card--done': !taskCard.status }"
            />
          </li>
        </transition-group>
      </ul>
    </div>
  </div>
</template>

<script>
import gsap from "gsap";
import InputForm from "./input-form.vue";
import TaskCard from "./task-card.vue";

export default {
  name: "Todolist",
  components: {
    InputForm,
    TaskCard,
  },

  data() {
    return {
      taskCards: [
        {
          id: Date.now(),
          name: "Мурзик",
          text: "Накормить кота",
          status: true,
        },
      ],
      taskCardsDone: [],
    };
  },
  methods: {
    createTask(taskCard) {
      this.taskCards.push(taskCard);
    },
    removeTask(task) {
      this.taskCards = this.taskCards.filter((t) => t.id !== task.id);
      this.taskCardsDone = this.taskCardsDone.filter((t) => t.id !== task.id);
    },
    addDone(task) {
      if (task.status == true) {
        task.status = false;
        this.taskCardsDone.push(task);
        this.taskCards = this.taskCards.filter((t) => t.id !== task.id);
      } else {
        task.status = true;
        this.taskCards.push(task);
        this.taskCardsDone = this.taskCardsDone.filter((t) => t.id !== task.id);
      }
    },
    // ============================Ainmations============================
    beforeEnterTask(el) {
      el.style.opacity = 0;
      el.style.height = 0;
      el.style.transform = "translateX(100%) scale(0.9)";
    },
    enterTask(el, done) {
      let tl = gsap.timeline();
      tl.to(el, {
        height: "auto",
        duration: 0.2,
        ease: "none",
      })
        .to(el, {
          opacity: 1,
          x: 0,
          duration: 0.2,
          ease: "circ",
        })
        .to(el, {
          scale: 1,
          duration: 0.2,
          ease: "bounce",
        })
        .to(el, { onComplete: done });
    },
    leaveTask(el, done) {
      let tl = gsap.timeline();
      tl.to(el, {
        scale: 0.9,
        duration: 0.2,
        ease: "bounce.out",
      })
        .to(el, {
          opacity: 0,
          x: "-100%",
          duration: 0.2,
          ease: "circ",
        })
        .to(el, {
          border: 0,
          height: 0,
          duration: 0.2,
          ease: "circ",
        })
        .to(el, { onComplete: done });
    },
    beforeEnterStatus(el) {
      el.style.opacity = 0;
      el.style.height = 0;
      el.style.marginTop = 0;
    },
    enterStatus(el, done) {
      let tl = gsap.timeline();
      tl.to(el, {
        opacity: 1,
        height: "1em",
        marginTop: 40,
        duration: 0.2,
        ease: "circ",
      }).to(el, { onComplete: done });
    },
    leaveStatus(el, done) {
      let tl = gsap.timeline();
      tl.to(el, {
        opacity: 0,
        duration: 0.2,
        marginTop: 0,
        height: 0,
        ease: "circ",
      }).to(el, { onComplete: done });
    },
    // ===========================/Ainmations============================
  },
};
</script>
