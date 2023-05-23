@@ -0,0 +1,105 @@
<template>
  <div>
    <h1>在线考试</h1>
    <el-card v-for="exam in exams" :key="exam.id">
      <h3>{{ exam.name }}</h3>
      <p>{{ exam.time }}</p>
      <el-button @click="startExam(exam.id)">进入考试</el-button>
    </el-card>

    <div v-if="currentExam">
      <h2>考试界面</h2>
      <div v-for="question in currentExam.questions" :key="question.id">
        <h3>{{ question.text }}</h3>
        <div v-if="question.type === 'multiple-choice'">
          <el-radio-group v-model="selectedAnswers[question.id]">
            <el-radio :label="option.id" v-for="option in question.options" :key="option.id">
              {{ option.text }}
            </el-radio>
          </el-radio-group>
        </div>
        <div v-else-if="question.type === 'true-false'">
          <el-radio-group v-model="selectedAnswers[question.id]">
            <el-radio label="true">True</el-radio>
            <el-radio label="false">False</el-radio>
          </el-radio-group>
        </div>
      </div>

      <el-button @click="submitExam">提交试卷</el-button>

      <div v-if="showAnswers">
        <h3>答案：</h3>
        <div v-for="question in currentExam.questions" :key="question.id">
          <p>
            {{ question.text }} - 正确答案: {{ question.answer }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ElButton, ElCard, ElRadio, ElRadioGroup } from 'element-ui';

export default {
  components: {
    ElButton,
    ElCard,
    ElRadio,
    ElRadioGroup
  },
  data() {
    return {
      exams: [
        {
          id: 1,
          name: '考试1',
          time: '2023-05-24 10:00:00',
          questions: [
            {
              id: 1,
              text: '判断题：1 + 1 = 2',
              type: 'true-false',
              answer: 'true'
            },
            {
              id: 2,
              text: '单选题：下列哪个不是编程语言？',
              type: 'multiple-choice',
              options: [
                { id: 1, text: 'Java' },
                { id: 2, text: 'HTML' },
                { id: 3, text: 'Python' },
                { id: 4, text: 'C++' }
              ],
              answer: 2
            }
          ]
        }
      ],
      currentExam: null,
      selectedAnswers: {},
      showAnswers: false
    };
  },
   methods: {
    startExam(examId) {
      // 根据考试ID获取考试信息并开始考试
      this.currentExam = this.exams.find(exam => exam.id === examId);
    },
    submitExam() {
      // 提交试卷
      this.showAnswers = true;
    }
  }
};
</script>

<style>
/* 根据需要自定义样式 */
.exam-card {
  margin-bottom: 20px;
}
</style>