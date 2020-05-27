<template>
  <div class="tree-item">
    <div class="tree-header">
      <div
        style="display: inline-block; width: 24px; height: 24px; text-align: center;"
        v-show="$slots.children"
      >
        <i
          class="fas"
          :class="{'fa-caret-down': showChildren, 'fa-caret-right': !showChildren}"
          @click="toggleChildren"
        ></i>
      </div>
      <div style="display: inline-block; height: 24px;">
        <slot></slot>
      </div>
    </div>
    <div class="children" v-if="$slots.children && showChildren">
      <slot name="children"></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showChildren: true
    };
  },
  methods: {
    toggleChildren() {
      this.showChildren = !this.showChildren;
    }
  }
};
</script>

<style lang="scss" scoped>
.tree-item {
  margin: 0;

  .children {
    padding-left: 24px;
  }

  .tree-header {
    height: 24px;
    line-height: 24px;
    // display: flex;
    // align-items: center;

    // i {
    //   height: 24px;
    //   width: 24px;
    // }

    // .expand {
    //   position: absolute;
    //   top: 50%;
    //   left: 50%;
    // }
  }
}
</style>
