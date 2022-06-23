<template>
  <div id="app">
    <transition name="fade" mode="out-in">
      <router-view/>
    </transition>
  </div>
</template>

<style lang="scss">
body {
  margin: 0;
}
#app {
  .note, .home {
    height: 100vh;
    display: flex;
    flex-direction: column;
    .main-block {
      flex: 1 0 87%;
      margin-top: 15px;
      border: 1px solid lightgray;
      .title-label {
        display: flex;
        width: 100%;
      }
    }
    .require {
      flex: 0 0 8%;
      width: 100%;
      color: #2c3e50;
      span {
        float: right;
        padding-right: 10px;
      }
    }
  }
  .edit, .remove {
    cursor: pointer;
    font-size: 1rem;
    padding: 5px 10px;
    border: none;
    border-radius: 10%;
    color: whitesmoke;
    transition: all 0.3s ease-out;
  }
  .remove {
    background-color: lightcoral;
    &:hover {
      background-color: darkred;
    }
  }
  .edit {
    margin-right: 5px;
    background-color: green;
    &:hover {
      background-color: darkgreen;
    }
  }
  a {
    text-decoration: none;
    color: cornflowerblue;
  }
  .head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-right: 10px;
    padding-left: 10px;
    .btn-block {
      display: flex;
      align-items: center;
      button {
        border-radius: 3px;
      }
      span {
        display: block;
        margin-top: -4px;
      }
    }
    h1 {
      text-shadow: 2px 2px 1px lightgrey;
    }
    .add {
      cursor: pointer;
      font-size: 2rem;
      line-height: 26px;
      height: fit-content;
      background-color: cornflowerblue;
      color: whitesmoke;
      border: none;
      border-radius: 10%;
      transition: all 0.3s ease-out;
      &:hover {
        background-color: darkslateblue;
      }
    }
  }
  width: 50%;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.empty {
  text-align: center;
  font-size: 18px;
}
.tasks {
  flex-direction: column;
  padding-inline-start: 20px;
  list-style-type: none;
  padding-left: 0;
  li {
    position: relative;
    word-break: break-all;
    padding-right: 10px;
    &:nth-child(3):after {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      content: '';
      width: 100%;
      z-index: 20;
      background: linear-gradient(rgba(255, 255, 255, 0.4), white);
    }
  }
}
.notes {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid lightgray;
  padding: 0 10px;
  &:last-child {
    border-bottom: 1px solid lightgray;
  }
  .action-note {
    display: flex;
    justify-content: end;
    flex: 1 0 28%;
  }
  .note-section {
    flex: 1 0 72%;
    .note-title {
      font-size: 24px;
      margin-bottom: 10px;
      margin-top: 10px;
      word-break: break-all;
    }
  }
}
.title-input {
  border: none;
  outline: none;
  font-size: 24px;
  width: 100%;
  margin-top: 30px;
  margin-bottom: 15px;
  padding: 0 10px;
  caret-color: cornflowerblue;
  color: #777377;
}
.modal {
  .dialog {
    color: black;
    background: whitesmoke;
    padding: 30px;
    border-radius: 5px;
    .dialog-actions {
      display: flex;
      justify-content: end;
      button {
        color: #323233;
        font-weight: 700;
        border: none;
        padding: 10px 15px;
        border-radius: 5px;
        transition: all 0.2s ease-out;
        &:first-child {
          background-color: lightgreen;
          margin-right: 5px;
        }
        &:last-child {
          background-color: lightgray;
        }
        &:first-child {
          &:hover {
            background-color: darkgreen;
            color: white;
          }
        }
        &:last-child {
          &:hover {
            background-color: darkgrey;
            color: white;
          }
        }
      }
    }
  }
}
@media only screen and (max-width : 768px) {
  #app {
    width: 90%;
    .dialog {
      width: 218px;
    }
    .title-input {
      margin-top: 10px;
      margin-bottom: 5px;
    }
    h1 {
      margin-top: 5px;
      margin-bottom: 5px;
    }
    .task {
      .task-input {
        width: 90%;
      }
    }
  }
}
.tip {
  left: 0;
  top: 0;
  text-align: center;
  font-size: 16px;
  position: fixed;
  width: 100%;
  padding: 15px;
  background-color: cornflowerblue;
  color: white;
}
.done {
  text-decoration: line-through 2px #3d4148;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .2s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
