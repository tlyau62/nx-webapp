<template>
  <div class="jstree">
    <slot></slot>
  </div>
</template>

<script>
import $ from "jquery";
import "jstree/dist/jstree";
import JstreeCtxMenu from "@/components/table-ctx-menu/JstreeCtxMenu";
import CtxMenuEvtBus from "nex-ctxmenu/src/components/ctx-menu/ctx-menu-evt-bus";

export default {
  props: ["options"],
  mounted() {
    $(this.$el).jstree(this.options || {});

    $(this.$el).bind("contextmenu.jstree", event => {
      event.preventDefault();
      event.stopPropagation();
      var node = $(event.target).closest("li");
      var data = node.data("jstree");
      const treeDim = window.document
        .querySelector("html")
        .getBoundingClientRect();

      CtxMenuEvtBus.$emit("open", {
        menuComp: JstreeCtxMenu,
        pos: {
          x: event.originalEvent.x - treeDim.left,
          y: event.originalEvent.y - treeDim.top
        },
        container: window.document.querySelector("html")
      });
      // Do some action
    });
  }
};
</script>

<style lang="scss" src="@/scss/jstree/jstree.scss"></style>