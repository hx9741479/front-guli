<template>
  <div class="app-container">
    <el-form label-width="120px">
      <el-form-item label="信息描述">
        <el-tag type="info">excel模版说明</el-tag>
        <el-tag>
          <i class="el-icon-download"/>
          <a :href="defaultExcelTemplate">点击下载模版</a>
        </el-tag>
      </el-form-item>

      <el-form-item label="选择Excel ">
        <el-upload
          ref="upload"
          :auto-upload="false"
          :on-exceed="fileUploadExceed"
          :on-success="fileUploadSuccess"
          :on-error="fileUploadError"
          :limit="1"
          list-type="picture"
          action="http://127.0.0.1:8110/admin/edu/subject/import"
          name="file"
          accept="application/vnd.ms-excel">
          <el-button
            slot="trigger"
            size="small"
            type="primary">选取文件</el-button>
          <el-button
            :disabled="importBtnDisabled"
            style="margin-left: 10px;"
            size="small"
            type="success"
            @click="submitUpload">导入</el-button>
        </el-upload>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      defaultExcelTemplate: process.env.OSS_PATH + '/excel/课程分类列表模板.xls', // 默认Excel模板
      importBtnDisabled: false // 导入按钮是否禁用
    }
  },
  methods: {
    // 上传多于一个视频时
    fileUploadExceed() {
      this.$message.warning('请先删除列表中的Excel文件')
    },

    // 上传
    submitUpload() {
      this.importBtnDisabled = true
      this.$refs.upload.submit() // 提交上传请求
    },

    // 成功回调
    fileUploadSuccess(response) {
      this.importBtnDisabled = false
      // 此回调响应不经过request.js中的响应拦截器
      this.$message({
        type: 'success',
        message: response.message
      })
      this.$refs.upload.clearFiles() // 清空已上传的文件列表
    },

    // 失败回调
    fileUploadError() {
      this.importBtnDisabled = false
      this.$message({
        type: 'error',
        message: '导入失败！'
      })
      this.$refs.upload.clearFiles() // 清空已上传的文件列表
    }
  }
}
</script>
