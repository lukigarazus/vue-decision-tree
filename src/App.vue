<template>
  <div id="app">
    <Layout>
      <Sider collapsible v-model="collapsed">
        <Menu theme="dark" :defaultSelectedKeys="[tab]" mode="inline">
          <Item v-for="item in menu" :key="item.value" @click="tab = item.value">
            <Icon :type="item.icon"/>
            <span>{{ item.label }}</span>
          </Item>
          <SubMenu key="trees" style="background: #9e1068">
            <div slot="title">
              <Icon type="bars"/>
              <span>Trees</span>
            </div>
            <Item v-if="!trees.length">
              <span></span>
              <span>No trees defined!</span>
            </Item>
            <Item v-for="(tree, index) in trees" :key="tree.id" @click="tab = index">
              <span>{{ tree.name }}</span>
            </Item>
          </SubMenu>
        </Menu>
      </Sider>
      <Content>
        <!-- <div v-if="tab === `dash`">Dashboard</div> -->
        <DecisionTreeCreation v-if="tab === 'tree'" :db="db" @create="refreshTrees"/>
        <DecisionTree v-else :tree="trees[tab]" :key="tab"/>
      </Content>
    </Layout>
  </div>
</template>

<script>
import "ant-design-vue/dist/antd.css";
import { Layout, Menu, Icon } from "ant-design-vue";
import Dexie from "dexie";
import DecisionTreeCreation from "./components/DecisionTreeCreation";
import DecisionTree from "./components/DecisionTree";

const { Content, Header, Sider } = Layout;
const { Item, SubMenu } = Menu;

export default {
  name: "App",
  components: {
    DecisionTreeCreation,
    Content,
    Header,
    Sider,
    Menu,
    Item,
    Layout,
    Icon,
    SubMenu,
    DecisionTree
  },
  data() {
    return {
      tab: "tree",
      collapsed: true,
      menu: [
        // { label: "Dashboard", value: "dash", icon: "question-circle" },
        { label: "Create a tree", value: "tree", icon: "edit" }
      ],
      db: new Dexie("decision"),
      trees: []
    };
  },
  methods: {
    async refreshTrees() {
      this.trees = await this.db.trees.toArray();
    }
  },
  async created() {
    this.db.version(1).stores({
      trees: "++id, name, firstQuestion"
    });
    this.refreshTrees();
  }
};
</script>

<style scoped>
.app {
  height: 100vh;
}
.ant-layout {
  height: 100vh;
}
</style>
