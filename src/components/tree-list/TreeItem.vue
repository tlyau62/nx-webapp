<template>
  <div id="tree-item">
    <div>
      <button @click="toggleChildren">[+] {{showChildren}}</button>
      <slot></slot>
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
#tree-item {
  margin: 0;

  .children {
    padding-left: 20px;
  }
}
</style>
