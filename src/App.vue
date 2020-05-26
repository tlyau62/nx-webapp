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
      <div class="content">
        <div class="top-menu">
          <div class="menu-item">
            <i class="fas fa-search"></i>
          </div>
          <div class="menu-item">
            <avatar username="Jane Doe" :inline="true" :size="24" :rounded="false"></avatar>
          </div>
        </div>
        <div class="table-container" @contextmenu.prevent="$refs.menu.open($event)">
          <div class="actions">
            <div>
              <button>Test button</button>
            </div>
          </div>
          <vue-tabulator v-model="dados" :options="options" />
          <vue-context ref="menu">
            <li>
              <a href="#" @click.prevent="onClick($event.target.innerText)">Row Option 1</a>
            </li>
            <li>
              <a href="#" @click.prevent="onClick($event.target.innerText)">Row Option 2</a>
            </li>
          </vue-context>
        </div>
      </div>
      <!-- <div class="col-4">1</div>
      <div class="col-8">2</div>-->
    </div>
  </div>
</template>

<script>
import TreeList from "@/components/tree-list";
import Avatar from "vue-avatar";
import { TabulatorComponent } from "vue-tabulator";
import { VueContext } from "vue-context";
import $ from "jquery";

export default {
  components: {
    ...TreeList,
    Avatar,
    VueTabulator: TabulatorComponent,
    VueContext
  },
  data() {
    const self = this;

    return {
      options: {
        layout: "fitColumns",
        selectable: true,
        rowFormatter(row) {
          $(row.getElement()).on("contextmenu", evt => {
            evt.preventDefault();
            event.stopPropagation();
            self.$refs.menu.open(evt);
          });
        },
        columns: [
          {
            title: "Name",
            field: "name",
            sorter: "string",
            width: 400
          },
          {
            title: "Age",
            field: "age",
            sorter: "int",
            width: 200
          },
          {
            title: "",
            headerSort: false
          },
          {
            titleFormatter: function(cell, formatterParams) {
              return "<i class='fas fa-cog'></i>";
            },
            width: 10,
            headerSort: false
          }
        ]
      },
      dados: [
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Teste",
          age: 13
        },
        {
          name: "Testez",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        },
        {
          name: "Testezs",
          age: 13
        }
      ]
    };
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
    border-right: 1px solid $gray-300;

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
    display: flex;
    flex-direction: column;
    height: 100vh;

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
      }

      .tabulator-row {
        height: 24px !important;
        min-height: 24px !important;
        max-height: 24px !important;
        border: none;
      }

      .tabulator-row .tabulator-cell {
        padding: 0;
      }

      // .tabulator-header {
      //   height: 24px !important;
      //   min-height: 24px !important;
      //   max-height: 24px !important;
      // }

      .tabulator-tableHolder {
        background-image: url("assets/grid-bg.svg");
        background-attachment: local;
      }

      .tabulator-selected {
        background-color: $gray-200;
      }

      .tabulator-header {
        border-top: 0;
        border-bottom: 1px solid $gray-300;
      }

      // .tabulator-table {
      //   max-height: calc(100vh - 48px - 36px);
      //   overflow: auto !important;
      // }

      .actions {
        display: flex;
        height: 36px;
        align-items: center;
      }
    }
  }
}
</style>

<style src="@fortawesome/fontawesome-free/css/all.css"></style>
