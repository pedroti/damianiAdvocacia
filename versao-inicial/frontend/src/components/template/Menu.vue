<template>
  <aside class="menu" v-show="isMenuVisible">
    <div class="menu-filter">
      <i class="fa fa-search fa-lg"></i>
      <input
        type="text"
        placeholder="Digite para filtrar..."
        v-model="treeFilter"
        class="filter-field"
      />
    </div>
    <div>
      <Tree :data="treeData" :options="treeOptions" :filter="treeFilter" ref="tree">
        <span class="tree-text" slot-scope="{ node }">
          <template>
            <i :class="node.data.icon" class="mr-2"></i>
            {{ node.text }}
          </template>
        </span>
      </Tree>
    </div>
  </aside>
</template>

<script>
import { mapState } from "vuex";
import Tree from "liquor-tree";

export default {
  name: "Menu",
  components: { Tree },
  computed: mapState(["isMenuVisible"]),
  data: function() {
    return {
      treeFilter: "",
      treeData: this.getTreeData(),
      treeOptions: {
        filter: {
          emptyText: "Item não encontrado"
        }
      }
    };
  },
  methods: {
    getTreeData() {
      return Promise.resolve([
        {
          text: "Admininistração",
          data: { name: "adminPages", icon: "fa fa-cogs" }
        },
        {
          text: "Dashboard",
          data: { name: "home", icon: "fa fa-home" }
        },
        {
          text: "Agenda",
          data: { name: "agenda", icon: "fa fa-calendar" }
        },
          {
          text: "Cliente",
          data: { name: "customer", icon: "fa fa-users" }
        }
      ]);
    },
    onNodeSelect(node) {
      this.$router.push({
        name: node.data.name
      });
    }
  },
  mounted() {
    this.$refs.tree.$on("node:selected", this.onNodeSelect);
  }
};
</script>

<style>
.menu {
  grid-area: menu;
  background: linear-gradient(to right, #232526, #414345);

  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}

.menu a,
.menu a:hover {
  color: #fff;
  text-decoration: none;
}

.menu .tree-node.selected > .tree-content,
.menu .tree-node .tree-content:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.tree-arrow.has-child {
  filter: brightness(2);
}

.menu .menu-filter {
  display: flex;
  justify-content: center;
  align-items: center;

  margin: 20px;
  padding-bottom: 8px;
  border-bottom: 1px solid #aaa;
}

.menu .menu-filter i {
  color: #aaa;
  margin-right: 10px;
}

.menu input {
  color: #ccc;
  font-size: 1.3rem;
  border: 0;
  outline: 0;
  width: 100%;
  background: transparent;
}

.tree-filter-empty {
  color: #ccc;
  font-size: 1.3rem;
  margin-left: 20px;
}

.tree-text {
  color: #bbb;
}
</style>
