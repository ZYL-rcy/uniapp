<template>
  <view>
    <!-- 二手交易列表 -->
    <view v-for="item in itemList" :key="item.id">
      <text>{{ item.title }}</text>
      <text>{{ item.amount }}</text>
      <text>{{ item.author.name }}</text>
      <text>{{ formatTime(item.postTime) }}</text>
    </view>
    
    <!-- 二手交易详情页 -->
    <view v-show="detailVisible">
       <text>{{ currentDetail.title }}</text>
       <text>{{ currentDetail.content }}</text>
       <text>{{ currentDetail.amount }}</text>
       <text>{{ currentDetail.author.name }}</text>
       <text>{{ formatTime(currentDetail.postTime) }}</text>
       <text>{{ currentDetail.contact }}</text>
       <view v-if="isAuthor(currentDetail.author.uuid)">
         <button @click="deletePost(currentDetail.id)">删除帖子</button>
       </view>
    </view>
  </view>
</template>

<script>
import { reactive, onMounted } from 'vue'

export default {
  data() {
    return {
      itemList: [], // 二手交易列表数据
      currentDetail: {}, // 当前查看的二手交易详情
      detailVisible: false // 二手交易详情页显示状态
    }
  },
  methods: {
    // 获取二手交易列表数据
    async getList() {
      // 调用后端接口获取数据，将返回的数据赋值给itemList
      this.itemList = await api.getList()
    },
    // 获取二手交易详情
    async getDetail(id) {
      // 调用后端接口获取数据，将返回的数据赋值给currentDetail
      this.currentDetail = await api.getDetail(id)
      this.detailVisible = true
    },
    // 删除帖子
    async deletePost(id) {
      // 调用后端接口删除帖子
      await api.deletePost(id)
      // 删除成功后隐藏详情页并刷新列表
      this.detailVisible = false
      this.getList()
    },
    // 判断当前用户是否是帖子的作者
    isAuthor(uuid) {
      // 获取当前登录用户的uuid，此处假设存储在userUuid变量中
      const userUuid = localStorage.getItem('userUuid')
      return uuid === userUuid
    },
    // 格式化时间
    formatTime(timestamp) {
      const date = new Date(timestamp)
      return date.toLocaleString()
    }
  },
  mounted() {
    // 组件加载完成时获取二手交易列表数据
    this.getList()
  }
}
</script>