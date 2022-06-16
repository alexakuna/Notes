<template>
  <div class="home">
    <div class="head">
      <h1>Notes</h1>
      <router-link
        :to="{name: 'NoteView', params: { note: emptyNote }}"
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
            :to="{name: 'NoteView', params: { note: note }}"
            custom
            v-slot="{ navigate }"
          >
            <button class="edit" @click="navigate" role="link">&#128393;</button>
          </router-link>
          <button class="remove" @click="removeNote(index)">&#128465;</button>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data: () => ({
    emptyNote: {
      id: '',
      title: '',
      tasks: [],
    },
    notes: [
      {
        id: 1,
        title: 'learn Java Script',
        tasks: [
          { title: 'Types of data', status: false, id: 1 },
          { title: 'Classes', status: false, id: 2 },
          { title: 'Hoisting', status: true, id: 3 },
        ],
      },
      {
        id: 2,
        title: 'learn CSS',
        tasks: [
          { title: 'Flex BOX', status: false, id: 1 },
          { title: 'Pseudo classes', status: false, id: 2 },
          { title: 'Margin, padding', status: true, id: 3 },
          { title: 'Grids', status: false, id: 4 },
          { title: 'Positions', status: true, id: 5 },
        ],
      },
    ],
  }),
  methods: {
    removeNote(index) {
      this.notes.splice(index, 1);
    },
  },
};
</script>
