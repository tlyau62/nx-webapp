<template>
  <div class="jstree">
    <slot></slot>
  </div>
</template>

<script>
import $ from "jquery";
import "jstree/dist/jstree";
import "jstree/dist/themes/default/style.css";

export default {
  props: ["options"],
  mounted() {
    $(this.$el).jstree(this.options || {});
  }
};
</script>

<style></style>