<template>
  <div id="app">
    <Layout>
      <Sider collapsible v-model="collapsed">
        <Menu theme="dark" :defaultSelectedKeys="['1']" mode="inline">
          <Item key="dash">
            <Icon type="question-circle"/>
            <span>Dashboard</span>
          </Item>
          <Item key="create">
            <Icon type="edit"/>
            <span>Create a tree</span>
          </Item>
          <SubMenu key="trees" style="background: #10239e">
            <div slot="title">
              <Icon type="bars"/>
              <span>Trees</span>
            </div>
          </SubMenu>
        </Menu>
      </Sider>
      <Content>
        <DecisionTree v-if="tab === 'tree'"/>
      </Content>
    </Layout>
  </div>
</template>

<script>
import "ant-design-vue/dist/antd.css";
import { Layout, Menu, Icon } from "ant-design-vue";
import DecisionTree from "./components/DecisionTree";

const { Content, Header, Sider } = Layout;
const { Item, SubMenu } = Menu;

export default {
  name: "App",
  components: {
    DecisionTree,
    Content,
    Header,
    Sider,
    Menu,
    Item,
    Layout,
    Icon,
    SubMenu
  },
  data() {
    return {
      tab: "tree",
      collapsed: true
    };
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
