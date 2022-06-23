<template>
  <div class="note">
    <div class="main-block">
      <transition-group name="fade">
        <div class="tip" key="1" v-if="tip">Changes saved!</div>
        <ModalComponent key="2" v-if="isShowDialog">
          <div class="dialog">
            <h3>Do you want to delete the task?</h3>
            <div class="dialog-actions">
              <button @click="removeTask(indexForRemoveTask)">Yes</button>
              <button @click="isShowDialog = false">Cancel</button>
            </div>
          </div>
        </ModalComponent>
        <ModalComponent key="3" v-if="isModalLeavePage">
          <div class="dialog">
            <h3>There is unsaved data on the page. Leave the page?</h3>
            <div class="dialog-actions">
              <button @click="confirm">Yes</button>
              <button @click="cancel">Cancel</button>
            </div>
          </div>
        </ModalComponent>
      </transition-group>
      <label class="title-label">
        <input
          ref="title-input"
          class="title-input"
          v-model="modelNote.title"
          type="text"
          placeholder="Enter name of note*"
        >
      </label>
      <div class="head" style="padding-bottom: 10px;">
        <router-link to="/">&#8678; Back</router-link>
        <div class="btn-block">
          <button
            key="1"
            v-if="indexUnReDo"
            class="edit" @click="unDo"
          >
            <span>&#11178;</span>
          </button>
          <button
            key="2"
            v-if="states.length - 1 > indexUnReDo"
            class="edit"
            @click="reDo"
          >
            <span>&#11179;</span>
          </button>
          <button
            key="3"
            v-if="canSave"
            class="edit" @click="saveNote"
          >
            Save
          </button>
          <button title="Add task" class="add" @click="addTask">+</button>
        </div>
      </div>
      <hr v-if="!modelNote.tasks.length">
      <div v-if="!modelNote.tasks.length" class="empty">
        There are no tasks yet. Click the button in the upper right corner.
      </div>
      <template v-else>
        <TaskComponent
          v-for="(task, index) in modelNote.tasks"
          :task="task"
          :key="task.id"
          @removeTask="defineShowDialog(index)"
          @status="changeStatusTask(index)"
          @title="changeTitleTask($event, index)"
        />
      </template>
    </div>
    <div class="require">
      <hr>
      * Required
    </div>
  </div>
</template>

<script>
import TaskComponent from '@/components/TaskComponent.vue';
import ModalComponent from '@/components/ModalComponent.vue';
import isEqual from 'lodash.isequal';

export default {
  name: 'NoteView',
  props: ['note', 'forEdit'],
  data: () => ({
    tip: false,
    edit: null,
    indexUnReDo: 0,
    isModalLeavePage: false,
    isShowSaveBtn: false,
    isShowDialog: false,
    indexForRemoveTask: null,
    isShowBtnFromTitle: false,
    isShowBtnFromTaskTitle: false,
    isShowBtnFromTasksLength: false,
    resolvePromise: '',
    rejectPromise: '',
    maskNote: null,
    states: [],
    modelNote: { id: '', title: '', tasks: [] },
  }),
  components: {
    TaskComponent,
    ModalComponent,
  },
  created() {
    // Получаем данные с localstorage.
    const note = localStorage.getItem('note');
    const edit = localStorage.getItem('edit');
    const states = localStorage.getItem('states');
    const index = localStorage.getItem('index');
    // Проверяем мы хотим создать или отредактировать заметку.
    // И делаем инициализацию первоначального состояния.
    if (this.forEdit) {
      this.edit = !!this.forEdit;
      this.modelNote = JSON.parse(JSON.stringify(this.note));
      this.maskNote = JSON.parse(JSON.stringify(this.note));
      localStorage.setItem('note', JSON.stringify(this.note));
      localStorage.setItem('edit', this.forEdit);
      this.states.push(JSON.parse(JSON.stringify(this.note)));
    } else {
      if (note !== null) {
        this.modelNote = JSON.parse(note);
        this.maskNote = JSON.parse(note);
      } else {
        this.maskNote = JSON.parse(JSON.stringify(this.modelNote));
      }
      if (edit !== null) {
        this.edit = edit;
      } else {
        this.edit = !!this.forEdit;
        localStorage.setItem('edit', this.edit);
      }
      if (index !== null) {
        this.indexUnReDo = +index;
      }
      if (states !== null) {
        this.states = JSON.parse(states);
      } else {
        this.states.push(JSON.parse(JSON.stringify(this.modelNote)));
      }
    }
  },
  computed: {
    // Все методы в этой секции проверяют условия на возможность сохранения данных.
    // Заметка не может быть сохранена если обязательные поля пустые.
    // Если зависимости изменились и условия соблюдены то можно сохраить.
    isShowBtn() {
      return this.isShowSaveBtn || this.isShowBtnFromTitle
        || this.isShowBtnFromTaskTitle || this.isShowBtnFromTasksLength;
    },
    isTitle() {
      return !!this.modelNote.title;
    },
    isTitleTask() {
      return this.modelNote.tasks.some((task) => !!task.title !== true);
    },
    canSave() {
      return this.isShowBtn && this.isTitle && !this.isTitleTask;
    },
    // Все методы в этой секции проверяют условия на возможность сохранения данных.
  },
  methods: {
    // Инициализируем отмену последнего сохранения.
    unDo() {
      if (this.indexUnReDo === 0) return;
      this.indexUnReDo -= 1;
    },
    // Отменяем отмену последнего последнего сохранения.
    reDo() {
      if (this.indexUnReDo === this.states.length - 1) return;
      this.indexUnReDo += 1;
    },
    // Подтверждение отмены редактирования. Последние изменения не сохранены.
    confirm() {
      this.isModalLeavePage = false;
      this.resolvePromise(true);
    },
    // Отменяем отмену редактирования.
    cancel() {
      this.isModalLeavePage = false;
      this.resolvePromise(false);
    },
    // Определение метода для обработки диалогового окна.
    dialog(next) {
      this.isModalLeavePage = true;
      next(false);
      return new Promise((resolve, reject) => {
        this.resolvePromise = resolve;
        this.rejectPromise = reject;
      });
    },
    // Добавить задачу.
    addTask() {
      this.modelNote.tasks.push({ title: '', status: false, id: Date.now() });
    },
    // Удалить задачу.
    removeTask(index) {
      this.modelNote.tasks.splice(index, 1);
      this.isShowDialog = false;
    },
    // Определение индекса удаляемой задачи.
    defineShowDialog(index) {
      this.indexForRemoveTask = index;
      this.isShowDialog = true;
    },
    // Меняем статус задачи выполнено/не выполнено.
    changeStatusTask(index) {
      const task = { ...this.modelNote.tasks[index], status: !this.modelNote.tasks[index].status };
      this.modelNote.tasks.splice(index, 1, task);
      this.isShowSaveBtn = this.maskNote.tasks
        .some((i, y) => i.status !== this.modelNote.tasks[y].status);
    },
    // Меняем название задачи.
    changeTitleTask(newTitle, index) {
      const task = this.modelNote.tasks[index];
      this.modelNote.tasks.splice(index, 1, { ...task, title: newTitle });
      this.isShowBtnFromTaskTitle = this.maskNote.tasks
        .some((i, y) => i.title !== this.modelNote.tasks[y]?.title);
    },
    // Метод сохранения заметки.
    saveNote() {
      const toSave = isEqual(this.modelNote, this.states[this.indexUnReDo]);
      if (!toSave) {
        this.states.push(this.modelNote);
        this.indexUnReDo = this.states.length - 1;
      }
      this.maskNote = JSON.parse(JSON.stringify(this.modelNote));
      localStorage.setItem(
        'note',
        JSON.stringify(
          { ...this.maskNote, id: this.edit ? this.maskNote.id : Date.now() },
        ),
      );
      localStorage.setItem('states', JSON.stringify(this.states));
      localStorage.setItem('index', JSON.stringify(this.indexUnReDo));
      this.isShowSaveBtn = false;
      this.isShowBtnFromTitle = false;
      this.isShowBtnFromTaskTitle = false;
      this.isShowBtnFromTasksLength = false;
      this.tip = true;
      setTimeout(() => {
        this.tip = false;
      }, 2000);
    },
  },
  mounted() {
    this.$refs['title-input'].focus();
  },
  watch: {
    'modelNote.title'(newValue) {
      this.isShowBtnFromTitle = this.maskNote.title !== newValue;
    },
    'modelNote.tasks.length'() {
      this.isShowBtnFromTasksLength = this.modelNote.tasks.length > this.maskNote.tasks.length
        || this.modelNote.tasks.length < this.maskNote.tasks.length;
    },
    states() {
      this.modelNote = { ...JSON.parse(JSON.stringify(this.states[this.indexUnReDo])) };
    },
    indexUnReDo(newValue) {
      if (newValue < 0 || newValue > this.states.length) return;
      this.modelNote = { ...JSON.parse(JSON.stringify(this.states[newValue])) };
      this.isShowSaveBtn = this.maskNote.tasks
        .some((task, index) => task.status !== this.modelNote.tasks[index]?.status);
    },
  },
  async beforeRouteLeave(to, from, next) {
    if (this.canSave) {
      const status = await this.dialog(next);
      if (!status) {
        this.isModalLeavePage = false;
        next(false);
      } else {
        next();
      }
    } else {
      next();
    }
    localStorage.removeItem('edit');
    localStorage.removeItem('states');
    localStorage.removeItem('index');
  },
};
</script>
