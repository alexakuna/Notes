<template>
  <div class="note">
    <h1>{{ emptyNote.title || 'Enter name of note' }}</h1>
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
      <TaskComponent v-for="task in emptyNote.tasks" :task="task" :key="task.id"/>
    </template>
  </div>
</template>

<script>
import TaskComponent from '@/components/TaskComponent.vue';

export default {
  name: 'NoteView',
  props: ['note'],
  data: () => ({
    emptyNote: null,
  }),
  components: {
    TaskComponent,
  },
  created() {
    if (!this.note) {
      this.$router.back();
    } else {
      this.emptyNote = this.note;
    }
  },
  methods: {
    addTask() {
      console.log('test');
      this.emptyNote.tasks.push({ title: '', status: false, id: Date.now() });
    },
    saveNote() {
      localStorage.setItem('notes', JSON.stringify(this.emptyNote));
    },
  },
};
</script>
