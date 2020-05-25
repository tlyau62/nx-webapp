<template>
  <div id="app">
    <div id="icons-panel">
      <div class="icon active">
        <i class="fas fa-home"></i>
      </div>
      <div class="icon">
        <i class="fas fa-box"></i>
      </div>

      <div class="bottom-icons">
        <div class="icon">
          <i class="fas fa-trash-alt"></i>
        </div>
        <div class="icon">
          <i class="fas fa-cogs"></i>
        </div>
      </div>
    </div>
    <div id="main-panel">
      <div class="left-menu">
        <div class="header">Repository</div>
        <div>
          <div class="subheader">My repositories</div>
          <div>
            <tree-list>
              <tree-item>
                Item1
                <template #children>
                  <tree-item>
                    Item1a
                    <template #children>
                      <tree-item>Item1aa</tree-item>
                      <tree-item>Item1ab</tree-item>
                    </template>
                  </tree-item>
                  <tree-item>Item1b</tree-item>
                </template>
              </tree-item>

              <tree-item>
                Item2
                <template #children>
                  <tree-item>Item2a</tree-item>
                </template>
              </tree-item>
            </tree-list>
          </div>
        </div>
      </div>
      <div class="content">content</div>
      <!-- <div class="col-4">1</div>
      <div class="col-8">2</div>-->
    </div>
  </div>
</template>

<script>
import TreeList from "@/components/tree-list";

export default {
  components: {
    ...TreeList
  }
};
</script>

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
    font-size: 20px;
    opacity: 60%;
    color: theme-color("light");
  }

  .icon {
    display: flex;
    width: 48px;
    height: 48px;
    align-items: center;
    justify-content: center;

    &.active {
      border-left: 2px solid color("cyan");
    }
  }
}

#main-panel {
  flex: 1;
  display: flex;

  .left-menu {
    width: 256px;
    line-height: 36px;
    background-color: $gray-100;

    .header {
      background-color: color("red");
      color: color("white");
      text-align: center;
    }

    .subheader {
      font-size: 15px;
      line-height: 36px;
      color: $gray-600;
      padding: 0 10px 0 10px;

      .action {
        float: right;
      }
    }
  }

  .content {
    flex: 1;
  }
}
</style>

<style src="@fortawesome/fontawesome-free/css/all.css"></style>
