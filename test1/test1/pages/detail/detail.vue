<template>
  <div class="container">
    <div class="header">课程表</div>
    <div class="table">
      <div class="row">
        <div class="cell time">课节</div>
        <div class="cell day">星期一</div>
        <div class="cell day">星期二</div>
        <div class="cell day">星期三</div>
        <div class="cell day">星期四</div>
        <div class="cell day">星期五</div>
        <div class="cell day">星期六</div>
        <div class="cell day">星期日</div>
      </div>
      <!-- 添加课程表数据 -->
      <div class="row" v-for="(item, index) in courseData" :key="index">
        <div class="cell time">{{ item.time }}</div>
        <div
          v-for="(day, dayIndex) in item.days"
          class="cell day"
          :key="dayIndex"
          :style="getCellStyle(day)"
        >
          {{ day }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courseData: [
        {
          time: "1-2",
          days: ["军事理论", "", "", "大学物理A1", "高等数学A2", "", ""],
        },
        {
          time: "3-4",
          days: ["中国现代史纲要", "大学物理A1", "高等数学A2", "体育2", "大学外语2", "", ""],
        },
        {
          time: "5-6",
          days: ["高等数学A2", "大学外语2", "线性代数A", "面向对象程序设计", "形势与政策2", "", ""],
        },
        {
          time: "7-8",
          days: [
            "面向对象程序设计",
            "计算机组装与维护",
            "大学生职业生涯与发展规划",
            "Web前端开发技术",
            "线性代数A",
            "",
            "",
          ],
        },
        {
          time: "9-10",
          days: ["", "音乐鉴赏", "影视鉴赏", "", "", "", ""],
        },
        {
          time: "11-12",
          days: ["", "", "", "", "", "", ""],
        },
        {
          time: "13-14",
          days: ["", "", "", "", "", "", ""],
        },
        // 添加其他时间段的数据
      ],
    };
  },
  methods: {
    getCellStyle(day) {
      switch (day) {
        case "高等数学A2":
          return { background: "#658f69" };
        case "大学物理A1":
          return { background: "#658f61" };
        case "面向对象程序设计":
          return { background: "#658f66" };
        case "军事理论":
          return { background: "#658f6c" };
        case "大学外语2":
          return { background: "#a6c090" };
        default:
          return { background: "white" };
      }
    },
  },
};
</script>

<style>
 .container { 
	   padding: 20rpx;
	   background-color: #f7f7f7;
	   border-radius: 10rpx;
	   box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1); 
		}

.header { 
	font-size: 36rpx; 
	font-weight: bold; 
	text-align: center; 
	margin-bottom: 20rpx; 
	color: #333; 
	background-color: #eee; 
	padding: 10rpx; 
	border-radius: 5rpx; }

.table { 
	display: flex; 
	flex-direction: column; 
	margin-top: 20rpx; 
	}

.row { display: flex; }

.cell { 
	flex: 1; 
	text-align: center; 
	border: none; 
	padding: 10rpx; 
	background-color: #fff; 
	border-radius: 5rpx; 
	box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1); 
	margin: 5rpx; }

.time { 
	font-weight: bold;
	 background-color: #eee; }

.day { background-color: #eee; }
</style>