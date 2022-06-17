<template>
  <div class="note">
    <ModalComponent v-if="showDialog">
      <div class="dialog">
        <h3>Do you want to delete the task?</h3>
        <div class="dialog-actions">
          <button @click="removeTask(indexForRemoveNote)">Yes</button>
          <button @click="showDialog = false">Cancel</button>
        </div>
      </div>
    </ModalComponent>
    <label>
      <input
        ref="title-input"
        class="title-input"
        v-model="emptyNote.title"
        type="text"
        placeholder="Enter name of note"
      >
    </label>
    <div class="head">
      <router-link to="/">&#8678; Back</router-link>
      <div style="display: flex; align-items: center">
        <button
          v-if="emptyNote.tasks.length"
          style="border-radius: 3px"
          class="edit" @click="saveNote"
        >Save</button>
        <button title="Add task" class="add" @click="addTask">+</button>
      </div>
    </div>
    <hr v-if="!emptyNote.tasks.length">
    <div v-if="!emptyNote.tasks.length" class="empty">
      There are no tasks yet. Click the button in the upper right corner.
    </div>
    <template v-else>
      <TaskComponent
        v-for="(task, index) in emptyNote.tasks"
        :task="task" :key="task.id"
        @removeTask="defineShowDialog(index)"
        @status="changeStatus(index)"
        @title="changeTitle($event, index)"
      />
    </template>
  </div>
</template>

<script>
import TaskComponent from '@/components/TaskComponent.vue';
import ModalComponent from '@/components/ModalComponent.vue';

export default {
  name: 'NoteView',
  props: ['note', 'forEdit'],
  data: () => ({
    showDialog: false,
    indexForRemoveNote: null,
    emptyNote: {
      id: '',
      title: '',
      tasks: [],
    },
  }),
  components: {
    TaskComponent,
    ModalComponent,
  },
  created() {
    if (this.note) {
      this.emptyNote = this.note;
    }
  },
  methods: {
    addTask() {
      this.emptyNote.tasks.push({ title: '', status: false, id: Date.now() });
    },
    removeTask(index) {
      this.emptyNote.tasks.splice(index, 1);
      this.showDialog = false;
    },
    defineShowDialog(index) {
      this.indexForRemoveNote = index;
      this.showDialog = true;
    },
    changeStatus(index) {
      this.emptyNote.tasks.forEach((task, idx) => {
        if (index === idx) {
          this.emptyNote.tasks.splice(index, 1, { ...task, status: !task.status });
        }
      });
    },
    changeTitle(newTitle, index) {
      const task = this.emptyNote.tasks.find((i, idx) => index === idx);
      this.emptyNote.tasks.splice(index, 1, { ...task, title: newTitle });
    },
    saveNote() {
      localStorage.setItem(
        'note',
        JSON.stringify(
          { ...this.emptyNote, id: this.forEdit ? this.emptyNote.id : Date.now() },
        ),
      );
      this.$router.push('/');
    },
  },
  mounted() {
    if (!this.emptyNote.tasks.length) this.$refs['title-input'].focus();
  },
};
</script>
