<template>
  <div class="container">
    <h1 class="text-center mb-3">---  期中考出題設定  ---</h1>
    <Form v-slot="{ errors }" @submit="onSubmit" class="row">
      <!--//* 教師資料 -->
      <div class="col-6 mb-3">
        <h2>教師資料</h2>
        <label for="name" class="form-label">姓名：</label>
        <Field id="name" name="姓名" type="text"
          class="form-control" :class="{ 'is-invalid': errors['姓名'] }"
          placeholder="請輸入姓名" rules="required" v-model="form.name"
        ></Field>
        <Error-message name="姓名" class="invalid-feedback"></Error-message>
        <label for="email" class="form-label">信箱：</label>
        <Field id="email" name="信箱" type="email"
          class="form-control" :class="{ 'is-invalid': errors['信箱'] }"
          placeholder="請輸入信箱" rules="email|required" v-model="form.email"
        ></Field>
        <Error-message name="信箱" class="invalid-feedback"></Error-message>
      </div>
      <div class="col-6">
        <!--//* 範圍設定 -->
        <div class="mb-3 p-2" :class="{ 'is-invalid': errors['範圍設定'] }">
          <h2>範圍設定</h2>
          <Field type="checkbox" name="範圍設定" rules="required" value="自然" v-model="form.range"
            :class="{ 'is-invalid': errors['範圍設定'] }" id="自然" />
          <label for="自然" class="form-label">自然</label>
          <Field type="checkbox" name="範圍設定" rules="required" value="生物" v-model="form.range"
            :class="{ 'is-invalid': errors['範圍設定'] }" id="生物" />
          <label for="生物" class="form-label">生物</label>
          <Field type="checkbox" name="範圍設定" rules="required" value="數學" v-model="form.range"
            :class="{ 'is-invalid': errors['範圍設定'] }" id="數學" />
          <label for="數學" class="form-label">數學</label>
          <Error-message name="範圍設定" class="invalid-feedback"></Error-message>
        </div>
        <!--//* 考卷類型 -->
        <div class="mb-3 p-2" :class="{ 'is-invalid': errors['考卷類型'] }">
          <h2>考卷類型</h2>
          <Field type="radio" name="考卷類型" rules="required" value="選擇題"
            :class="{ 'is-invalid': errors['考卷類型'] }" id="選擇題" v-model="form.type" />
          <label for="選擇題" class="form-label">選擇題</label>
          <Field type="radio" name="考卷類型" rules="required" value="克漏字"
            :class="{ 'is-invalid': errors['考卷類型'] }" id="克漏字" v-model="form.type" />
          <label for="克漏字" class="form-label">克漏字</label>
          <Field type="radio" name="考卷類型" rules="required" value="複選題"
            :class="{ 'is-invalid': errors['考卷類型'] }" id="複選題" v-model="form.type" />
          <label for="複選題" class="form-label">複選題</label>
          <Field type="radio" name="考卷類型" rules="required" value="作文題"
            :class="{ 'is-invalid': errors['考卷類型'] }" id="作文題" v-model="form.type" />
          <label for="作文題" class="form-label">作文題</label>
          <Error-message name="考卷類型" class="invalid-feedback"></Error-message>
        </div>
      </div>
      <!--//* 日期 -->
      <div class="col-4 mb-4" :class="{ 'is-invalid': errors['檔案'] }">
        <h2>報考日期</h2>
        <section>
          <DatePicker v-model:value="testDate" confirm="true" confirm-text="確認修改"
          type="date" range placeholder="選擇可報考日期"
          :disabled-date="notBeforeToday"
          :disabled-time="notBeforeTodayTwelveOClock"
          :clearable="false"
          class="mb-2"
          :class="{'is-invalid': errorShow['testDate'] === true}"
          value-type="format"
          ></DatePicker>
          <br>
          <small class="text-danger" v-if="errorShow['testDate'] === true">報考日期 為必填</small>
        </section>
        <h2>選擇時段</h2>
        <DatePicker
          :time-picker-options="{
            start: '08:30',
            step: '00:30',
            end: '18:30',
          }"
          :clearable="false"
          format="hh:mm a"
          type="time"
          placeholder="選擇時段"
          v-model:value="time"
          :class="{'is-invalid': errorShow['time'] === true}"
          value-type="format"
        ></DatePicker>
        <br>
        <small class="text-danger" v-if="errorShow['time'] === true">選擇時段 為必填</small>
      </div>

      <!-- //* 上傳 -->
      <div class="col-4">
        <Upload></Upload>
      </div>
      <div class="col-4" :class="{ 'is-invalid': errors['檔案'] }">
        <h2>補充圖片上傳(<span class="text-danger">必填</span>)</h2>
        <Field type="file" name="檔案"
        :rules="{ mimes: ['image/jpeg','image/png','image/jpg','image/svg','image/gif'], required: true }" :class="{ 'is-invalid': errors['檔案'] }"
          id="field" @change="showImg" />
        <Error-message name="檔案" class="invalid-feedback"></Error-message>
        <ul>
          <li class="mt-2">使用套件驗證</li>
          <li>可使用類型：<code>jpg | svg | jpeg | png | gif</code></li>
        </ul>
        <img :src="fileImgUrl" alt="預覽圖片" width="300" v-if="fileImgUrl">
      </div>
      <hr>

      <button
        class="btn me-2 btn-outline-primary"
        type="submit">驗證
      </button>
    </Form>
  </div>
  <hr>
  <!--//* 檢查資料 -->
  <h2 class="text-center mb-3">---  檢查資料  ---</h2>
  <ul class="row">
    <li class="col-3 mb-2">
      <h3>教師資料</h3>
      <p>姓名：{{ form.name }}</p>
      <p>信箱：{{ form.email }}</p>
    </li>
    <li class="col-3 mb-2">
      <h3>考卷範圍設定</h3>
      考試範圍：<strong v-for="item in form.range" :key="item" class="mb-1">{{ item }},</strong>
    </li>
    <li class="col-3">
      <h3>考卷類型</h3>
      <p>類型：{{ form.type}}</p>
    </li>
    <li class="col-3">
      <h3>報考日期</h3>
      <p v-if="testDate.length > 0"> {{ testDate[0] }} ~ {{ testDate[1] }} </p>
      <h3>選擇時段</h3>
      <p> {{time}} </p>
    </li>
  </ul>
</template>

<script>
import Upload from '@/views/UploadView.vue'
import DatePicker from 'vue-datepicker-next'
import 'vue-datepicker-next/index.css'
import 'vue-datepicker-next/locale/zh-cn'
export default {
  components: {
    DatePicker,
    Upload
  },
  data () {
    return {
      form: {
        name: '',
        email: '',
        range: [],
        type: ''
      },
      testDate: [],
      time: '',
      errorShow: {},
      fileImgUrl: ''
    }
  },
  methods: {
    onSubmit () {
      this.check()
    },
    notBeforeToday (date) {
      return date < new Date(new Date().setHours(0, 0))
    },
    notBeforeTodayTwelveOClock (date) {
      return date < new Date(new Date().setHours(12, 0))
    },
    check () {
      this.checkTestDate()
      this.checkTime()
    },
    checkTestDate () {
      if (this.testDate.length === 0) {
        this.errorShow.testDate = true
      } else {
        this.errorShow.testDate = false
      }
    },
    checkTime () {
      if (this.time === '') {
        this.errorShow.time = true
      } else {
        this.errorShow.time = false
      }
    },
    showImg (e) {
      const file = e.target.files[0]
      this.fileImgUrl = URL.createObjectURL(file)
    }
  }
}
</script>
<style scoped>
.is-invalid {
  border: 1px solid red;
  border-radius: 0.35rem;
}
</style>
