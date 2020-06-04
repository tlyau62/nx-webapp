<template>
  <ctx-menu class="icon-ctx-menu">
    <ctx-menu-item>
      <i class="fas fa-info-circle"></i>Info
    </ctx-menu-item>
    <ctx-menu-item>
      <i class="far fa-trash-alt"></i>Delete
    </ctx-menu-item>
    <ctx-menu-item>
      <i class="fas fa-edit"></i>Edit
    </ctx-menu-item>
    <ctx-menu-item>
      <i class="fas fa-file-download"></i>
      Label
      <ctx-menu>
        <div class="labelContainer">
          <div class="label" v-for="label in labels" :key="label" :style="{background: label}"></div>
        </div>
      </ctx-menu>
    </ctx-menu-item>
    <ctx-menu-separator />
    <ctx-menu-item>
      <i class="fas fa-file-download"></i>
      Download
      <ctx-menu>
        <ctx-menu-item>As zip</ctx-menu-item>
        <ctx-menu-item>
          As 7z
          <ctx-menu>
            <ctx-menu-item>High compress ratio</ctx-menu-item>
            <ctx-menu-item>Normal compress ratio</ctx-menu-item>
            <ctx-menu-item>Low compress ratio</ctx-menu-item>
          </ctx-menu>
        </ctx-menu-item>
        <ctx-menu-item>Normally</ctx-menu-item>
      </ctx-menu>
    </ctx-menu-item>
    <ctx-menu-item>
      <i class="fas fa-location-arrow"></i>
      Workflow
      <ctx-menu>
        <ctx-menu-item>Workflow item 1</ctx-menu-item>
        <ctx-menu-item>Workflow item 2</ctx-menu-item>
        <ctx-menu-item>Workflow item 3</ctx-menu-item>
      </ctx-menu>
    </ctx-menu-item>
  </ctx-menu>
</template>

<script>
import CtxMenu from "nex-ctxmenu/src/components/ctx-menu";

export default {
  name: "TableCtxMenu",
  components: {
    ...CtxMenu
  },
  data() {
    return {
      labels: new Array(20).fill(0).map(e => this.getRandomColor())
    };
  },
  methods: {
    getRandomColor() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    }
  }
};
</script>

<style scoped lang="scss">
@import "~bootstrap";

.icon-ctx-menu {
  i {
    padding-right: 10px;
    color: $gray-500;
  }

  .labelContainer {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 0 5px;

    .label {
      display: inline-block;
      width: 25px;
      height: 25px;
      margin: 5px;

      &:hover {
        @extend .shadow;
      }
    }
  }
}

.ctx-menu {
}
</style>
