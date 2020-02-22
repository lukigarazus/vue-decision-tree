<template>
  <div class="decision-tree">
    <h2>{{ tree.name }}</h2>
    <Breadcrumb class="horizontal-margin">
      <BItem v-for="(question, index) in questionStack" :key="index">
        <span class="breadcrumb-item" @click="onBreadcrumbClick(index)">{{ question.question }}</span>
      </BItem>
    </Breadcrumb>
    <div class="question">
      <h4>{{ currentQuestion.question }}</h4>
    </div>
    <div
      class="options"
      :style="{ flexDirection: currentQuestion.options.length > 4 ? `column` : `` }"
    >
      <div
        v-for="(option, index) in currentQuestion.options"
        :key="index"
        @click="changeCurrentQuestion(option.value)"
        class="option"
      >
        <span>{{ option.name }}</span>
      </div>
    </div>
  </div>
</template>


<script lang="ts">
import { Card, Input, Button, Icon, Breadcrumb } from "ant-design-vue";

const { Item } = Breadcrumb;

export default {
  props: ["db", "tree"],
  data() {
    return {
      currentQuestion: this.tree.firstQuestion,
      questionStack: [this.tree.firstQuestion]
    };
  },
  methods: {
    onBreadcrumbClick(index) {
      this.questionStack = this.questionStack.slice(0, index + 1);
      this.currentQuestion = this.questionStack[this.questionStack.length - 1];
    },
    changeCurrentQuestion(question) {
      this.currentQuestion = question;
      this.questionStack.push(question);
    }
  },
  components: {
    Card,
    Input,
    Button,
    Icon,
    Breadcrumb,
    BItem: Item
  }
};
</script>

<style scoped>
.decision-tree {
  padding: 10px;
}
.question {
  display: flex;
  justify-content: center;
  margin: 15px 0 0 0;
}
.options {
  display: flex;
  justify-content: space-around;
}
.option {
  cursor: pointer;
  width: 100%;
  border: 1px solid #e8e8e8;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  background: white;
  padding: 5px;
}
.option:hover {
  filter: brightness(0.85);
}
.breadcrumb-item {
  cursor: pointer;
}
</style>
