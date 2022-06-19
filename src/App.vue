<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
body {
  margin: 0;
}
#app {
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
    padding: 10px;
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
  width: 70%;
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
}
.notes {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid black;
  padding: 0 10px;
  &:last-child {
    border-bottom: 1px solid black;
  }
  .note {
    .note-title {
      font-size: 24px;
      margin-bottom: 10px;
      margin-top: 10px;
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
@media (min-width: 320px) and (max-width: 425px) {
  #app {
    width: 90%;
    .dialog {
      width: 218px;
    }
    .title-input {
      margin-top: 20px;
      margin-bottom: 0;
    }
    h1 {
      margin-top: 5px;
      margin-bottom: 5px;
    }
  }

}
</style>
