<template>
  <div class="home">
    <ModalComponent v-if="showDialog">
      <div class="dialog">
        <h3>Do you want to delete the note?</h3>
        <div class="dialog-actions">
          <button @click="removeNote(indexForRemoveNote)">Yes</button>
          <button @click="showDialog = false">Cancel</button>
        </div>
      </div>
    </ModalComponent>
    <div class="head">
      <h1>Notes</h1>
      <router-link
        :to="{name: 'NoteView', params: { note: emptyNote, forEdit: false }}"
        custom
        v-slot="{ navigate }"
      >
        <button class="add" @click="navigate" role="link">+</button>
      </router-link>
    </div>
    <hr v-if="!notes.length">
    <div v-if="!notes.length" class="empty">
      There are no notes yet. Click the button in the upper right corner.
    </div>
    <template v-else>
      <div class="notes" v-for="(note, index) in notes" :key="note.id">
        <div class="note">
          <div class="note-title">{{ note.title }}</div>
          <ol class="tasks">
            <li v-for="task in note.tasks.slice(0, 3)" :key="task.id">
              {{ task.title }}
            </li>
          </ol>
        </div>
        <div class="action-note">
          <router-link
            :to="{name: 'NoteView', params: { note: note, forEdit: true }}"
            custom
            v-slot="{ navigate }"
          >
            <button class="edit" @click="navigate" role="link">&#128393;</button>
          </router-link>
          <button class="remove" @click="defineShowDialog(index)">&#128465;</button>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import ModalComponent from '@/components/ModalComponent.vue';

export default {
  name: 'HomeView',
  data: () => ({
    emptyNote: {
      id: '',
      title: '',
      tasks: [],
    },
    notes: [],
    showDialog: false,
    indexForRemoveNote: null,
  }),
  components: {
    ModalComponent,
  },
  created() {
    const notes = localStorage.getItem('notes');
    if (notes !== null) {
      this.notes = JSON.parse(notes);
    }
    const note = localStorage.getItem('note');
    if (note !== null) {
      const parsedNote = JSON.parse(note);
      this.notes.forEach((i, index) => {
        if (parsedNote.id === i.id) {
          this.notes.splice(index, 1, parsedNote);
        }
      });
      if (!this.notes.some((i) => parsedNote.id === i.id)) {
        this.notes.push(parsedNote);
      }
      localStorage.removeItem('note');
      localStorage.setItem('notes', JSON.stringify(this.notes));
    }
  },
  methods: {
    defineShowDialog(index) {
      this.indexForRemoveNote = index;
      this.showDialog = true;
    },
    removeNote(index) {
      this.notes.splice(index, 1);
      this.showDialog = false;
      localStorage.setItem('notes', JSON.stringify(this.notes));
    },
  },
};
</script>
