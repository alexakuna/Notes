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
    padding-top: 10px;
    padding-bottom: 10px;
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
}
.notes {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid black;
  .note {
    .note-title {
      font-size: 24px;
      margin-bottom: 10px;
      margin-top: 10px;
    }
  }
}
@media (min-width: 320px) and (max-width: 425px) {
  #app {
    width: 90%;
    h1 {
      font-size: 20px;
    }
  }

}
</style>
