<template>
  <div class="jstree">
    <slot></slot>
  </div>
</template>

<script>
import $ from "jquery";
import "jstree/dist/jstree";
import "@/scss/jstree.scss";

export default {
  props: ["options"],
  mounted() {
    $(this.$el).jstree(this.options || {});
  }
};
</script>

<style></style>