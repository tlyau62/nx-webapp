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
          <div class="subheader">
            My repositories
            <div class="action">
              <i class="fas fa-filter"></i>
            </div>
          </div>
          <div>
            <js-tree :options="jstreeOptions">
              <ul>
                <li>
                  Folder A
                  <ul>
                    <li>SubFolder A</li>
                    <li>
                      SubFolder B
                      <ul>
                        <li>
                          SubSubFolder C
                          <ul>
                            <li>SubFolder A</li>
                            <li>SubFolder B</li>
                          </ul>
                        </li>
                      </ul>
                    </li>
                  </ul>
                </li>
              </ul>
            </js-tree>
          </div>
        </div>
      </div>
      <div class="content">
        <div class="top-menu">
          <div class="menu-item">
            <i class="fas fa-bars"></i>
          </div>
          <div class="menu-item">
            <i class="fas fa-search"></i>
          </div>
          <div class="menu-item" @click="showAvatar">
            <avatar username="Jane Doe" :inline="true" :size="24" :rounded="false"></avatar>
          </div>
        </div>
        <div class="table-toolbar">
          <div class="table-nav">
            <i class="fas fa-cloud"></i>
            <i class="fas fa-chevron-right"></i>
            <span style="color: #666666">FolderA</span>
            <i class="fas fa-chevron-right"></i>
            <span style="color: #666666">SubFolderB</span>
          </div>
          <span style="flex: 1;"></span>
          <div class="table-actions">
            <span>
              <img src="@/assets/newfolder.png" />
              New Folder
            </span>
            <span>
              <img src="@/assets/uploadfile.png" />
              Upload File
            </span>
            <span>
              <img src="@/assets/uploadfolder.png" />
              Upload Folder
            </span>
          </div>
        </div>
        <div class="table-container">
          <div class="tabulator-wrapper">
            <vue-tabulator ref="tabulator" v-model="dados" :options="options" />
            <div class="tabulator-status-bar" v-if="selectedRowInfo.length > 0">
              <div>
                <b style="font-weight: 500">{{selectedRowInfo.length}} / {{dados.length}}</b> items are selected, with total size of
                <b style="font-weight: 500">? MB</b>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="col-4">1</div>
      <div class="col-8">2</div>-->
    </div>

    <ctx-menu-factory></ctx-menu-factory>
  </div>
</template>

<script>
import TreeList from "@/components/tree-list";
import Avatar from "vue-avatar";
import { TabulatorComponent } from "vue-tabulator";
import { VueContext } from "vue-context";
import $ from "jquery";
import JsTree from "@/components/js-tree";
import CtxMenuFactory from "nex-ctxmenu/src/components/ctx-menu/CtxMenuFactory";
import CtxMenuEvtBus from "nex-ctxmenu/src/components/ctx-menu/ctx-menu-evt-bus";
import TableCtxMenu from "@/components/table-ctx-menu/TableCtxMenu";
import AvatarMenu from "@/components/table-ctx-menu/AvatarMenu";

window.$ = $;

export default {
  components: {
    ...TreeList,
    Avatar,
    VueTabulator: TabulatorComponent,
    VueContext,
    ...JsTree,
    CtxMenuFactory,
    CtxMenuEvtBus,
    TableCtxMenu
  },
  data() {
    const self = this;

    return {
      jstreeOptions: {
        plugins: ["wholerow"]
      },
      lastSelectedRow: null,
      selectedRowInfo: [],
      options: {
        layout: "fitColumns",
        selectable: true,
        rowFormatter(row) {
          const $el = $(row.getElement());
          const $action = $el.find(".cell-action");

          $el.on("contextmenu", evt => {
            const tableEl = row.getTable().element;
            const tableDim = tableEl.getBoundingClientRect();
            evt.preventDefault();
            event.stopPropagation();
            console.log(tableDim);
            console.log(evt);
            CtxMenuEvtBus.$emit("open", {
              menuComp: TableCtxMenu,
              pos: {
                x: evt.originalEvent.x - tableDim.left,
                y: evt.originalEvent.y - tableDim.top
              },
              container: tableEl
            });
          });

          $el.data("rowdata", row);

          $el.mouseenter(evt => $action.removeClass("d-none"));
          $el.mouseleave(evt => $action.addClass("d-none"));
        },
        rowClick(e, row) {
          const { ctrlKey, shiftKey } = e;

          if (ctrlKey) {
            self.lastSelectedRow = row;
            self.selectedRowInfo = row.getTable().getSelectedRows();
            return;
          }

          if (shiftKey) {
            if (self.lastSelectedRow === null) {
              return;
            }

            const selected = self.$refs.tabulator
              .getInstance()
              .getSelectedRows();

            window.table = self.$refs.tabulator.getInstance();

            for (const srow in selected) {
              selected[srow].deselect();
            }

            let min, max;

            if (
              $(self.lastSelectedRow.getElement()).index() <
              $(row.getElement()).index()
            ) {
              min = self.lastSelectedRow.getElement();
              max = row.getElement();
            } else {
              max = self.lastSelectedRow.getElement();
              min = row.getElement();
            }

            min = $(min);
            max = $(max);

            while (!min.is(max)) {
              min.data("rowdata").select();
              min = min.next();
            }
            max.data("rowdata").select();

            self.selectedRowInfo = row.getTable().getSelectedRows();
            return;
          }

          const selected = self.$refs.tabulator.getInstance().getSelectedRows();

          for (const srow in selected) {
            if (selected[srow].getElement() != row.getElement()) {
              selected[srow].deselect();
            }
          }

          self.lastSelectedRow = row;

          self.selectedRowInfo = row.getTable().getSelectedRows();
        },
        columns: [
          {
            titleFormatter: function(cell, formatterParams) {
              return "<div class='w-100 text-center'><i class='fas fa-lock'></i></div>";
            },
            formatter(cell, formatterParams, onRendered) {
              return cell.getValue()
                ? "<i class='fas fa-lock lock-icon'></i>"
                : "<span class='unlock-icon'>&bull;</span>";
            },
            width: 10,
            headerSort: false,
            align: "center",
            field: "locked"
          },
          {
            formatter(cell, formatterParams, onRendered) {
              return `<span style="display: flex; align-items: center;"><img src="folder.png" style="height: 14px; padding: 0 10px 0 0;"/>${cell.getValue()}</span>`;
            },
            title: "Name",
            field: "name",
            sorter: "string"
          },
          {
            title: "Size",
            field: "size",
            sorter: "string",
            width: 200
          },
          {
            title: "Type",
            field: "type",
            sorter: "string",
            width: 200
          },
          {
            title: "Date added",
            field: "added",
            sorter: "string",
            width: 200
          },
          {
            title: "",
            formatter(cell, formatterParams, onRendered) {
              const $btn = $(
                `<button class="cell-action d-none" ><i class="fas fa-ellipsis-h"></i></button>`
              );

              $btn.click(evt => {
                const tableEl = cell.getTable().element;
                const tableDim = tableEl.getBoundingClientRect();
                const targetDim = evt.target.getBoundingClientRect();

                evt.stopPropagation();
                CtxMenuEvtBus.$emit("open", {
                  menuComp: TableCtxMenu,
                  pos: {
                    x: targetDim.x - tableDim.left,
                    y: targetDim.y + targetDim.height - tableDim.top
                  },
                  container: tableEl
                });
              });

              return $btn[0];
            },
            align: "center",
            headerSort: false,
            width: 100
          },
          {
            titleFormatter: function(cell, formatterParams) {
              return "<div class='w-100 text-center'><i class='fas fa-cog'></i></div>";
            },
            width: 10,
            headerSort: false
          }
        ]
      },
      dados: [
        ...new Array(100).fill(0).map((v, i) => ({
          name:
            Math.random()
              .toString(36)
              .substring(7) + ".txt",
          size: Math.trunc(Math.random() * 10240) + " MB",
          type: "document",
          added: new Date(),
          locked: Math.random() > 0.5
        }))
      ]
    };
  },
  methods: {
    showAvatar(evt) {
      console.log("asdf");

      const targetDim = evt.target.getBoundingClientRect();

      CtxMenuEvtBus.$emit("open", {
        menuComp: AvatarMenu,
        // menuComp: require("@/components/table-ctx-menu/AvatarMenu"),
        pos: {
          x: targetDim.left,
          y: targetDim.top + targetDim.height
        },
        container: window.document.querySelector("html")
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/bootstrap.scss";
@import "~vue-context/src/sass/vue-context";
@import "~tabulator-tables/dist/css/bootstrap/tabulator_bootstrap4.css";

html,
body {
  height: 100%;
  width: 100%;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
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

    background-color: $gray-100;
    border-right: 1px solid $gray-300;

    .header {
      line-height: 36px;
      background-color: darken(#beebff, 40%);
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
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: calc(100vw - 48px - 256px);

    .top-menu {
      background-color: $gray-100;
      border-bottom: 1px solid $gray-300;
      height: 36px;
      min-height: 36px;
      display: flex;
      flex-direction: row-reverse;
      align-items: center;

      .menu-item {
        padding: 0 5px 0 5px;
        margin: 0 5px;
      }
    }

    .table-container {
      height: calc(100% - 36px - 36px);

      .tabulator,
      .tabulator-table,
      .tabulator-row {
        background: none;
      }

      .tabulator {
        height: 100%;
        // color: rgb(51, 51, 51);
      }

      .tabulator-row {
        height: 24px !important;
        min-height: 24px !important;
        max-height: 24px !important;
        border: none;

        &:hover {
          background-color: lighten($gray-200, 3%);
        }
      }

      .tabulator-row .tabulator-cell {
        height: 24px !important;
        min-height: 24px !important;
        max-height: 24px !important;
        padding: 0 10px 0 10px;
        font-weight: 400;
        font-size: $font-size-base * 1px;
        vertical-align: top;
      }

      .tabulator-tableHolder {
        background-image: url("assets/grid-bg.svg");
        background-attachment: local;
      }

      .tabulator-row.tabulator-selected {
        background-color: $gray-200;
      }

      .tabulator-header {
        border-top: 0;
        border-bottom: 1px solid $gray-300;
      }

      .tabulator-col {
        border-right: 1px solid $gray-300;
        height: 24px;

        .tabulator-col-content {
          padding: 0 10px 0 10px;
          font-size: $font-size-base * 1px;
          color: $gray-700;
          font-weight: 400;
        }
      }

      .actions {
        height: 36px;

        .right-actions {
          float: right;
        }

        button {
          display: inline-block;
          vertical-align: middle;
        }
      }

      .tabulator-wrapper {
        display: flex;
        flex-direction: column;
        height: 100%;
      }

      .lock-icon {
        color: $gray-700;
      }

      .unlock-icon {
        color: $gray-500;
      }

      .tabulator-arrow {
        top: calc(21px / 2);
      }
    }

    .table-toolbar {
      display: flex;
      flex-direction: row;
      align-content: center;
      height: 36px;

      .table-nav {
        margin-left: 5px;
        color: $gray-500;
        display: flex;
        align-items: center;

        & > * {
          margin: 0 5px;
        }
      }

      .table-actions {
        display: flex;
        align-items: center;

        & > * {
          margin: 0 5px;
        }
      }
    }

    .tabulator-status-bar {
      background-color: $gray-200;
      color: $gray-700;
      height: 36px;
      padding: 5px 10px;
      display: flex;
      align-content: center;
    }

    .cell-action {
      font-size: 10px;
      padding: 0 8px;
      vertical-align: middle;

      button {
        border-radius: 5px;
        border: 1px solid gray;
      }
    }
  }
}
</style>

<style src="@fortawesome/fontawesome-free/css/all.css"></style>
