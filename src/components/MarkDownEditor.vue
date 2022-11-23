<script setup lang="ts">
import editor from 'mavon-editor'
import 'mavon-editor/dist/css/index.css'
import { ref } from 'vue'
import { saveAs } from 'file-saver'
import { ElMessage } from 'element-plus'
import { genFileId } from 'element-plus'
import type { UploadInstance, UploadProps, UploadRawFile } from 'element-plus'


// 下载文件弹窗是否显示
const dialogVisible = ref(false)
// markdown 文档的内容
const markdownValue = ref('')
// markdown 文档名
const markdownName = ref('')
// 上传文件的实例
const upload = ref<UploadInstance>()

// 显示markdown 编辑器里面的内容
const markdownValueChange = (value: string, render: string) => {
  // console.log(value, render)
  // console.log(markdownValue.value)
}

// 关闭输入文件名字的弹窗,并下载文件
const closeDialog = () => {
  if (markdownName.value.trim().length < 1) {
    ElMessage({
      showClose: true,
      message: '输入的文件名长度 <1,请重新输入',
      type: 'warning'
    })
  } else if (markdownName.value.trim().length > 30) {
    ElMessage({
      showClose: true,
      message: '输入的文件名长度 >30,请重新输入',
      type: 'warning'
    })
  } else {
    dialogVisible.value = false
    const blob = new Blob([markdownValue.value])
    saveAs(blob, `${markdownName.value}.md`)
  }
}

// 文件超出上传数量
const handleExceed: UploadProps['onExceed'] = files => {
  upload.value!.clearFiles()
  const file = files[0] as UploadRawFile
  file.uid = genFileId()
  upload.value!.handleStart(file)
}

// 文件上传之前的检查：类型和大小
const beforeAvatarUpload: UploadProps['beforeUpload'] = async rawFile => {
  const rawFileName = rawFile.name
  const fileType = rawFileName !== '' ? rawFileName.split('.')[1] : ''
  if (fileType !== 'txt' && fileType !== 'md') {
    ElMessage.error('上传的文件格式应为 .md 或者 .txt')
    return false
  } else if (rawFile.size / 1024 / 1024 > 2) {
    ElMessage.error('文件大小不要超过 2MB!')
    return false
  }
  const text = await rawFile.text()
  markdownValue.value = text
  return true
}
</script>

<template>
  <editor.mavonEditor
    v-model="markdownValue"
    @change="markdownValueChange"
    @save="dialogVisible = true"
    class="mavon-editor"
  ></editor.mavonEditor>
  <div class="actions-btn">
    <el-button type="success" round @click="dialogVisible = true">下载文件</el-button>
    <el-upload
      ref="upload"
      class="upload-demo"
      :limit="1"
      :before-upload="beforeAvatarUpload"
      :on-exceed="handleExceed"
    >
      <template #trigger>
        <el-button type="warning" round>上传文件</el-button>
      </template>
    </el-upload>
  </div>

  <el-dialog v-model="dialogVisible" title="下载文件" width="30%">
    <el-input v-model="markdownName" placeholder="请输入文件名 1~30 位" :clearable="true">
      <template #append>.md</template>
    </el-input>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="closeDialog">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped lang="scss">
.upload-demo {
  margin-left: 20px;
  display: flex;
  align-items: center;
}
.mavon-editor {
  margin-bottom: 50px;
}
.v-note-wrapper {
  height: 650px;
}
.actions-btn {
  width: 100%;
  position: fixed;
  bottom: 0;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>
