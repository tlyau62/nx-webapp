<template>
  <div class="jstree">
    <slot></slot>
  </div>
</template>

<script>
import $ from "jquery";
import "jstree/dist/jstree";

export default {
  props: ["options"],
  mounted() {
    $(this.$el).jstree(this.options || {});
  }
};
</script>

<style lang="scss" src="@/scss/jstree/jstree.scss"></style>