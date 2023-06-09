<template>
  <div class="box">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>考试题库</span>
        <!-- <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button> -->
      </div>
      <div v-for="o in 4" :key="o" class="text item">
        <el-card class="card_item">
          <div slot="header" class="clearfix">
            <span class="title">软件工程考试题</span>
            <el-button style="float: right; padding: 3px 0" type="text" @click="beginexam"
              >开始考试</el-button
            >
          </div>
          <div class="wrap">
            <div class="tx">考试题型：单选题、判断题</div>
            <div class="time">考试期限：{{time}}</div>
            <div class="examtime">考试时间：20分钟</div>
          </div>
        </el-card>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
    data() {
        return {
            time:"2023/5/10-2023/6/10",
            paperid:"1"
        }
    },
    methods:{
        beginexam(){
            this.$router.push("/exam/${this.paperid}")
        }
    }
};
</script>

<style scoped>

.box {
  width: 100%;
  padding: 20px 0px;
}
.text {
  font-size: 14px;
}
.card_item {
    min-height: 150px;
}
.item {
  margin-bottom: 18px;
}
.wrap {
    display: flex;
    justify-content: space-around;
}
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}

.box-card {
  margin: 0px 200px;
  width: 80%;
  min-height: 820px;
}
.box-card /deep/ .el-card__header {
  background-color: rgb(141, 194, 240);
}
.card_item /deep/ .el-card__header{
    background-color: #fff;
}
.card_item /deep/ .el-card__header .clearfix {
    position: relative;
}
.card_item /deep/ .el-card__header .title {
    position: absolute;
    left: 2px;
    font-size: 16px;
}
</style>
