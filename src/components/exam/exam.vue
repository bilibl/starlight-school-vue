
<template>
  <div class="one">
    <div class="detail">
      <span style="font-size:25px;color='#333'">{{ exam.name }}</span>
      <!-- <span style="font-size:20px;color='#333'"></span> -->
    </div>
    <el-alert v-if="showAnswers" effect="dark" center > 本次得分为{{sumScore}}分,试题总分为{{totalScore}}分</el-alert>
    <el-alert v-else  effect="dark" center>考试剩余时间
          <el-statistic :value="deadline" time-indices @finish="timeover" format="HH:mm:ss" >
          </el-statistic>
    </el-alert>

    <div class="content">
      <div
        v-for="(item, index) in exam.questions"
        :key="item.id"
        class="contenStyle"
      >
        <div class="questionStyle">
          <div class="title">
            {{ index + 1 }}.{{ item.text }}
            <span v-if="item.type === 1" class="testFont">(单选题)</span>
            <span v-if="item.type === 2" class="testFont">(判断题)</span>
          </div>
          <!-- 选择题 -->
          <div v-if="item.type === 1" class="choice">
            <el-radio-group v-model="selectedAnswers[item.id]">
            <el-radio :label="option.text" v-for="option in item.options" :key="option.id">
              {{ option.text }}
            </el-radio>
          </el-radio-group>
          </div>
          <!-- 判断题 -->
          <div v-if="item.type === 2" class="choice">
           <el-radio-group v-model="selectedAnswers[item.id]">
              <el-radio label="true">True</el-radio>
              <el-radio label="false">False</el-radio>
          </el-radio-group>
          </div>
        </div>

        <!-- 正确答案 -->
        <el-card style="margin: 20px 200px;" v-if="showAnswers">
          <div>
            <div class="right" v-if="isAnswerCorrect(item)" style="color:green">
              正确答案：{{ item.answer }}，你选对了
            </div>
            <div class="wrong" v-else style="color:red">
              正确答案：{{ item.answer }}，你选错了
            </div>
          </div>
        </el-card>
      </div>
      <!-- 提交测验 -->
      <div>
        <el-button v-if="!showAnswers" @click="submitExam">提交试卷</el-button>
        <el-button v-else @click="again">再考一遍</el-button>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  props: ["paperid"],
  data() {
    return {
      deadline:Date.now() + 1000 * 60 * 20,
      
      exam: {
        id:1,
        name: "软件工程考试",
        time:"2023/5/10",
        questions: [
        {
          id:1,
          text: "具有风险分析的软件生命周期模型是( )",
          type: 1,
          options:[
            {id:1,text:"瀑布模型"},
            {id:2,text:"喷泉模型 "},
            {id:3,text:"螺旋模型"},
            {id:4,text:"增量模型"},
          ],
          answer:"螺旋模型"
        },
        {
          id:2,
          text: "批处理操作系统首先要考虑的问题是（    ）",
          type: 1,
          options:[
            {id:1,text:"灵活性和可适应性"},
            {id:2,text:"实时性和可靠性"},
            {id:3,text:"交互性和响应时间"},
            {id:4,text:"周转时间和系统吞吐量"},
          ],
          answer:"实时性和可靠性"
        },
        {
          id:3,
          text: "建立动态模型的第一步，是编写典型交互行为的脚本。",
          type: 2,
          answer:'true'
        },
        {
          id:4,
          text: "软件错误可能出现在开发过程的早期，越早修改越好。",
          type: 2,
          answer:'true'
        },
      ],
      },
      
      sumScore:"",
      totalScore:"",

      selectedAnswers: {},
      showAnswers: false
    };
  },
  created() {
      // this.getpaper(this.paperid)
  },
  methods: {
    //   获取试卷
    // getpaper(paperid) {
    //     getpaper({paperid})
    //     .then(res=>{
    //         // this.questions=res.data.paper
    //     })
    //     .catch(

    //     )
    // },
    isAnswerCorrect(question) {
      const selectedAnswer = this.selectedAnswers[question.id];
        // 判断题
        if (question.type === 2) {
          return selectedAnswer === question.answer.toString();
        }

        // 单选题
        if (question.type === 1) {
          return selectedAnswer === question.answer;
        }

        return false; // 默认返回错误
    },
      calculateScore() {
      let score = 0;
      for (const question of this.exam.questions) {
        if (this.isAnswerCorrect(question)) {
          score += 25; // 每题分
        }
      }
      return score;
    },
    submitExam() {
        this.showAnswers = true
        this.sumScore = this.calculateScore()
        this.totalScore = 25 * this.exam.questions.length
    },
    again() {
      location.reload();
    },
    timeover() {
      this.submitExam()
    }
  },
};
</script>

<style>
.one {
  /* height: 800px; */
  min-height: 750px;
  width: 100%;
  border-style: solid;
  border-width: 0.1px;
  border-radius: 10px;
  border-color: #edeeef;
  background-color: white;
  margin-left: 0px;
}
.detail {
  margin-top: 20px;
  margin-bottom: 30px;
  font-size: 28px;
  font-weight: bold;
}
.questionStyle {
  display: flex;
  flex-direction: column;
  margin: 30px 200px;
}
.title {
  display: flex;
  color: rgb(95, 92, 92);
  margin: 10px 0;
  font-size: 20px;
}
.choice {
  margin-top: 30px;
  font-size: 18px;
  display: flex;
  justify-content: space-between;
}
.testFont {
  font-size: 14px;
}
.el-radio__label {
  font-size: 18px !important;
}
.el-alert .el-alert__description {
    font-size: 18px;
    font-weight: bold;
}
.el-alert--info.is-dark {
    background-color: rgb(45,115,204);
}
</style>


/* 根据需要自定义样式 */
.exam-card {
  margin-bottom: 20px;
}
</style>