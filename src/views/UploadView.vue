<template>
  <div class="mb-2">
    <h2>圖片上傳</h2>
    <input type="file" @change="getFileInfo">
  </div>
  <div class="d-flex m-3 p-3">
    <ul class="me-4" v-if="status > 0">
      <li>上傳狀態：<span class="rounded-circle me-2 px-3 py-1"
        :class="`bg-${status===1? 'success': 'danger'}`"></span>
        <i v-if="status===uploadStatus.failSize" class="text-danger">*您的檔案超過限制的 "30MB"，您的檔案大小為 "{{ fileSizeMB }} MB"</i>
        <i v-if="status===uploadStatus.failFormat" class="text-danger">*您的檔案格式不正確，您上傳的是 "{{ fileType }}"</i>
      </li>
      <li v-if="status===1">上傳日期：{{ uploadDate }}</li>
      <li>檔案名稱：{{ fileName }}</li>
      <li class="mb-3">檔案大小：{{ fileSizeMB }} MB</li>
    </ul>
    <div>
      <img :src="fileImgUrl" alt="預覽圖片" width="300" v-if="status===1">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      status: 0,
      uploadStatus: {
        null: 0,
        success: 1,
        failFormat: 3,
        failSize: 4
      },
      isSuccess: false,
      uploadDate: '',
      fileName: '',
      fileType: '',
      fileTypeIsOk: '',
      fileSize: '',
      fileSizeMB: '',
      fileSizeIsOk: '',
      fileImgUrl: ''
    }
  },

  methods: {
    getFileInfo (e) {
      const imgType = ['jpg', 'png', 'svg', 'jpeg', 'gif']
      const file = e.target.files[0]

      //* 上傳的副檔名
      const lastValLen = file.name.split('.').length
      this.fileType = file.name.split('.')[lastValLen - 1]

      //* 若上傳資料夾，會無法顯示錯誤上傳的副檔名
      const isFolder = file.type === ''
      if (isFolder && lastValLen === 1) this.fileType = '文件夾'

      this.fileImgUrl = URL.createObjectURL(file)
      this.fileTypeIsOk = imgType.includes(this.fileType)
      this.fileSize = file.size
      this.fileSizeMB = (this.fileSize / 1024 / 1024).toFixed(2)
      this.fileName = file.name
      this.fileSizeIsOk = this.fileSize < 31457280
      this.checkFile()
    },
    checkFile () {
      if (!this.fileTypeIsOk) {
        this.uploadFail(this.uploadStatus.failFormat)
      } else if (!this.fileSizeIsOk) {
        this.uploadFail(this.uploadStatus.failSize)
      } else {
        this.uploadDate = new Date().getTime()
        this.status = 1
      }
    },
    uploadFail (failType) {
      this.status = failType
    }
  }

}
</script>
