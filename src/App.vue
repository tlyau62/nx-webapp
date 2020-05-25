<template>
  <div id="app">
    <div id="icons-panel">
      <i class="fas fa-home"></i>
      <i class="fas fa-box"></i>

      <div class="bottom-icons">
        <i class="fas fa-trash-alt"></i>
        <i class="fas fa-cogs"></i>
      </div>
    </div>
    <div id="main-panel">
      <p>test</p>
    </div>
  </div>
</template>

<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";

html,
body {
  height: 100%;
  width: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  display: flex;
  flex-direction: row;
}

#icons-panel {
  display: flex;
  flex-direction: column;
  width: 48px;
  background-color: theme-color("dark");

  .bottom-icons {
    @extend #icons-panel;
    position: absolute;
    bottom: 0;
  }

  i {
    display: flex;
    font-size: 20px;
    width: 48px;
    height: 48px;
    align-items: center;
    justify-content: center;
    color: theme-color("light");
    opacity: 60%;
  }
}
</style>

<style src="@fortawesome/fontawesome-free/css/all.css"></style>
