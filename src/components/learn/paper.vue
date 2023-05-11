<template>
  <div class="one">
    <div class="detail">
      <span style="font-size:25px;color='#333'">{{ exam.name }}</span>
      <!-- <span style="font-size:20px;color='#333'"></span> -->
    </div>
    <el-alert v-if="ischecked===1" effect="dark" center > 本次得分为{{sumScore}}分,试题总分为{{totalScore}}分</el-alert>
    <el-alert v-else  effect="dark" center> 温馨提示：请认真答题，切勿遗漏</el-alert>
    <div class="content">
      <div
        v-for="(item, index) in questions"
        :key="item.id"
        class="contenStyle"
      >
        <div class="questionStyle">
          <div class="title">
            {{ index + 1 }}.{{ item.name }}
            <span v-if="item.type === 1" class="testFont">(单选题)</span>
            <span v-if="item.type === 2" class="testFont">(判断题)</span>
          </div>
          <!-- 选择题 -->
          <div v-if="item.type === 1" class="choice">
            <span
              ><el-radio v-model="item.studentAnswer" label="A"
                >A.{{ item.a }}</el-radio
              ></span
            >
            <span
              ><el-radio v-model="item.studentAnswer" label="B"
                >B.{{ item.b }}</el-radio
              ></span
            >
            <span
              ><el-radio v-model="item.studentAnswer" label="C"
                >C.{{ item.c }}</el-radio
              ></span
            >
            <span
              ><el-radio v-model="item.studentAnswer" label="D"
                >D.{{ item.d }}</el-radio
              ></span
            >
          </div>
          <!-- 判断题 -->
          <div v-if="item.type === 2" class="choice">
            <span
              ><el-radio v-model="item['answer' + index]" label="是"
                >是{{ item.a }}</el-radio
              ></span
            >
            <span
              ><el-radio v-model="item['answer' + index]" label="否"
                >否{{ item.a }}</el-radio
              ></span
            >
          </div>
        </div>

        <!-- 正确答案 -->
        <el-card style="margin: 20px 200px;" v-if="ischecked === 1">
          <div>
            <div class="right" v-show="item.isright" style="color:green">
              正确答案：{{ item.answer }}，你选对了
            </div>
            <div class="wrong" v-show="!item.isright" style="color:red">
              正确答案：{{ item.answer }}，你选错了
            </div>
          </div>
        </el-card>
      </div>
      <!-- 提交测验 -->
      <div v-if="ischecked === 0">
        <el-divider></el-divider>
        <el-button @click="submitPaper">提交测验</el-button>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  props: ["course"],
  data() {
    return {
      exam: {
        name: "通用测验",
      },
      questions: [
        {
          name: "批处理操作系统首先要考虑的问题是（    ）",
          type: 1,
          answer: "B",
          studentAnswer:"",
          isright: true,
          a: "灵活性和可适应性",
          b: "实时性和可靠性",
          c: "交互性和响应时间",
          d: "周转时间和系统吞吐量",
        },
        {
          name: "批处理操作系统首先要考虑的问题是（    ）",
          type: 1,
          answer: "B",
          studentAnswer:"",
          isright: true,
          a: "灵活性和可适应性",
          b: "实时性和可靠性",
          c: "交互性和响应时间",
          d: "周转时间和系统吞吐量",
        },
        {
          name: "批处理操作系统首先要考虑的问题是（    ）",
          type: 1,
          answer: "B",
          studentAnswer:"",
          isright: true,
          a: "灵活性和可适应性",
          b: "实时性和可靠性",
          c: "交互性和响应时间",
          d: "周转时间和系统吞吐量",
        },
      ],
      ischecked: 0,
      sumScore:"8",
      totalScore:"8"
    };
  },
  created() {
      console.log(this.course.id);
      this.getpaper(this.course.id)
  },
  methods: {
    //   获取试卷
    getpaper(course_id) {
        getpapers({course_id})
        .then(res=>{
            // this.questions=res.data.paper
        })
        .catch(

        )
    },
    submitPaper() {
        console.log(this.questions);
    },
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
