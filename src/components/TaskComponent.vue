<template>
<div class="task">
  <div class="desc-block">
    <label>
      <input
        type="checkbox"
        :disabled="!isTitle"
        :checked="task.status"
        @change="changeStatus"
      >
    </label>
    <div class="description">
      <input
        class="task-input"
        ref="input"
        type="text"
        placeholder="Enter task*"
        :disabled="task.status"
        v-model="title"
        :class="{done: task.status}"
      >
    </div>
  </div>
  <div class="actions-block">
    <button @click="deleteTask" class="remove">&#128465;</button>
  </div>
</div>
</template>

<script>
export default {
  name: 'TaskComponent',
  props: ['task'],
  data: () => ({
    title: null,
  }),
  computed: {
    // Если название задачи не указано не можем изменить статус.
    isTitle() {
      return !!this.task.title;
    },
  },
  created() {
    this.title = this.task.title;
  },
  methods: {
    deleteTask() {
      this.$emit('removeTask');
    },
    changeStatus() {
      this.$emit('status');
    },
  },
  mounted() {
    this.$refs.input.focus();
  },
  watch: {
    title(newValue) {
      this.$emit('title', newValue);
    },
  },
};
</script>

<style lang="scss">
.done-block {
  position: absolute;
  width: 100%;
  height: 0.1rem;
  background-color: #595959;
  top: calc(50% - 0.1rem);
}
.task {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-top: solid 1px #d0c4c4;
  &:last-child {
    border-bottom: solid 1px #d0c4c4;
  }
  .desc-block {
    width: 90%;
    display: flex;
    justify-content: center;
    align-items: center;
    input[type=checkbox] {
      width: 1.2rem;
      height: 1.2rem;
      margin-right: 5px;
    }
    .description {
      width: 100%;
      position: relative;
      word-break: break-all;
      input {
        width: 100%;
        border: none;
        outline: none;
        padding: 10px;
        background-color: transparent;
        caret-color: cornflowerblue;
        color: #777377;
      }
    }
  }
}
</style>
