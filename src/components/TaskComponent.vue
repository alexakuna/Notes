<template>
<div class="task">
  <div class="desc-block">
    <label>
      <input type="checkbox" :checked="status" @change="changeStatus">
    </label>
    <div class="description">
      <div v-if="status" class="done-block"></div>
<!--      <span>{{ title }}</span>-->
      <input
        ref="input"
        type="text"
        placeholder="Enter task"
        :disabled="status"
        v-model="title"
      >
    </div>
  </div>
  <div class="actions-block">
<!--    <button class="edit" :disabled="status">&#128393;</button>-->
    <button @click="deleteTask" class="remove">&#128465;</button>
  </div>
</div>
</template>

<script>
export default {
  name: 'TaskComponent',
  props: ['task'],
  data: () => ({
    status: null,
    title: null,
  }),
  created() {
    this.status = this.task.status;
    this.title = this.task.title;
  },
  methods: {
    deleteTask() {
      this.$emit('removeTask');
    },
    changeStatus() {
      this.$emit('status');
      this.status = !this.status;
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
    display: flex;
    justify-content: center;
    align-items: center;
    input[type=checkbox] {
      width: 1.2rem;
      height: 1.2rem;
      margin-right: 5px;
    }
    .description {
      position: relative;
      word-break: break-all;
      input {
        border: none;
        outline: none;
        padding: 10px;
        background-color: transparent;
      }
    }
  }
}
</style>
