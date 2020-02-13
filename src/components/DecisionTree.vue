<template>
  <div class="decision-tree">
    <Modal
      title="Point to a question"
      :footer="null"
      :visible="optionModal"
      @cancel="optionModal = false"
    >
      <div class="modal-buttons">
        <Button type="primary" @click="createQuestion">Create a new question</Button>
        <Select
          :style="{ width: '240px' }"
          placeholder="Select a question"
          @select="selectQuestion"
        >
          <SOption
            v-for="(option, index) in questions"
            :key="index"
            :value="index"
          >{{ option.question }}</SOption>
        </Select>
      </div>
    </Modal>
    <Card hoverable title="Tree name">
      <Input v-model="tree.name"/>
    </Card>
    <Breadcrumb style="margin: 10px 0 10px 0">
      <BItem v-for="(question, index) in questionStack" :key="index">
        <span class="breadcrumb-item" @click="onBreadcrumbClick(index)">{{ question.question }}</span>
      </BItem>
    </Breadcrumb>
    <div class="question">
      <Card hoverable title="Question">
        <Input v-model="currentQuestion.question"/>
      </Card>
      <div :class="{ options: true, column: currentQuestion.options.length > 4 }">
        <Input
          v-for="(option, index) in currentQuestion.options"
          :key="index"
          v-model="option.name"
        >
          <div class="option-addon" slot="addonBefore" @click="changeCurrentQuestion(option.value)">
            <Icon type="left"/>
          </div>
          <div class="option-addon" slot="addonAfter" @click="deleteOption(index)">
            <Icon type="delete"/>
          </div>
        </Input>
        <Button type="primary" @click="addOption">
          <Icon type="plus"/>
        </Button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {
  Card,
  Input,
  Button,
  Icon,
  Breadcrumb,
  Modal,
  Select
} from "ant-design-vue";

const { Item } = Breadcrumb;
const { Option: SOption } = Select;

interface Option {
  name: string;
  value: Question;
}

interface Question {
  question: string;
  options: Option[];
}

export default {
  data() {
    const emptyQuestion1 = { question: "", options: [] };
    const emptyQuestion2 = { question: "", options: [] };
    const initialQuestion: Question = {
      question: "How do u do?",
      options: [
        { name: "Option 1", value: emptyQuestion1 },
        { name: "Option 2", value: emptyQuestion2 }
      ]
    };
    return {
      currentQuestion: initialQuestion,
      questionStack: [initialQuestion],
      tree: { name: "Tree", firstQuestion: initialQuestion },
      optionModal: false,
      questions: [initialQuestion, emptyQuestion1, emptyQuestion2]
    };
  },
  methods: {
    onBreadcrumbClick(index) {
      this.questionStack = this.questionStack.slice(0, index + 1)
      this.currentQuestion = this.questionStack[this.questionStack.length - 1]
    },
    addOption() {
      this.optionModal = true;
    },
    selectQuestion(value) {
      const question = this.questions[value];
      if (this.currentQuestion === question) {
        alert("You cannot point a question to itself!");
        return;
      }
      this.currentQuestion.options.push({
        name: `Option ${this.currentQuestion.options.length + 1}`,
        value: question
      });
      this.optionModal = false;
    },
    createQuestion() {
      const newQuestion = { question: "", options: [] };
      this.currentQuestion.options.push({
        name: `Option ${this.currentQuestion.options.length + 1}`,
        value: newQuestion
      });
      this.questions.push(newQuestion);
      this.optionModal = false;
    },
    deleteOption(index) {
      this.currentQuestion.options.splice(index, 1);
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
    BItem: Item,
    Modal,
    Select,
    SOption
  }
};
</script>

<style scoped>
.decision-tree {
  padding: 10px;
}
.options {
  display: flex;
}
.options.column {
  flex-direction: column;
}
.modal-buttons {
  display: flex;
  justify-content: space-around;
}
.option-addon {
  cursor: pointer;
}
.breadcrumb-item {
  cursor: pointer;
}
</style>
