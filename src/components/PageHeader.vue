<script setup lang="ts">
import { ref, watch } from 'vue'

const baseUrl = 'https://fuss10.elemecdn.com'
const urls = [
  `${baseUrl}/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg`,
  `${baseUrl}/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg`,
  `${baseUrl}/0/6f/e35ff375812e6b0020b6b4e8f9583jpeg.jpeg`,
  `${baseUrl}/9/bb/e27858e973f5d7d3904835f46abbdjpeg.jpeg`,
  `${baseUrl}/d/e6/c4d93a3805b3ce3f323f7974e6f78jpeg.jpeg`,
  `${baseUrl}/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg`,
  `${baseUrl}/2/11/6535bcfb26e4c79b48ddde44f4b6fjpeg.jpeg`
]
const body = document.getElementsByTagName('body')[0].clientWidth
const height = ref('125px')
</script>

<template>
  <div class="el-carousel-box">
    <el-carousel :interval="3000" type="card" :height="height" class="el-carousel">
      <el-carousel-item v-for="item in 7" :key="item">
        <el-image :src="urls[item - 1]" alt="图片未正常加载" fit="contain" />
      </el-carousel-item>
    </el-carousel>
  </div>
  <div class="title">欢迎使用西里奥的 MarkDown 编辑器</div>
</template>

<style scoped lang="scss">
.el-carousel-box {
  display: flex;
  justify-content: center;
  align-items: center;
}
.el-carousel {
  width: 355px;
  margin-top: 10px;
}
.title {
  text-align: center;
  font-size: 15px;
  color: #1a73e8;
  font-family: '正楷';
  font-weight: bold;
  line-height: 30px;
}

@media screen and (min-width: 600px) {
  .title {
    display: none;
  }
  .el-carousel {
    width: 580px;
  }
}
@media screen and (min-width: 800px) {
  .title {
    display: block;
    font-size: 25px;
    font-weight: bold;
    line-height: 50px;
    margin-bottom: 20px;
  }
  .el-carousel {
    margin-top: 10px;
    display: none;
  }
}
</style>
